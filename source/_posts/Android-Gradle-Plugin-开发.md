---
title: Android Gradle Plugin 开发
date: 2019-04-05 12:26:02
tags: gradle
---

Android 使用 Gradle 作为其构建脚本语言，与其强大的可扩展性密不可分。Android 官方依赖 gradle 开发了一个 `com.android.tools.build:gradle:x.x.x`库，这个库大家应该都不陌生，创建新项目的时候，会默认依赖这个库。这个库中包含了一些 Android 官方实现的 plugin 以及大量的 transform。

![](baseplugin.png)

和 Android 相关的 plugin 都继承自 `com.android.build.gradle.BasePlugin`。和 Android 无关的 plugin 直接实现`org.gradle.api.Plugin`接口。

这里以`AppPlugin`为例，其实对应的就是 application module 下的 build.gradle 中的 `apply plugin: 'com.android.application'`，点进去看看源码：

```java
protected BaseExtension createExtension(
        @NonNull Project project,
        @NonNull ProjectOptions projectOptions,
        @NonNull AndroidBuilder androidBuilder,
        @NonNull SdkHandler sdkHandler,
        @NonNull NamedDomainObjectContainer<BuildType> buildTypeContainer,
        @NonNull NamedDomainObjectContainer<ProductFlavor> productFlavorContainer,
        @NonNull NamedDomainObjectContainer<SigningConfig> signingConfigContainer,
        @NonNull NamedDomainObjectContainer<BaseVariantOutput> buildOutputs,
        @NonNull SourceSetManager sourceSetManager,
        @NonNull ExtraModelInfo extraModelInfo) {
    return project.getExtensions()
            .create(
                    "android",  // extension name
                    AppExtension.class,
                    project,
                    projectOptions,
                    androidBuilder,
                    sdkHandler,
                    buildTypeContainer,
                    productFlavorContainer,
                    signingConfigContainer,
                    buildOutputs,
                    sourceSetManager,
                    extraModelInfo);
}
```

可以看到在这个 plugin 下创建了一个`android`的扩展，`android`对应的就是 build.gradle 中的 `android {...}`。

从1.5.0-beta1开始，Android gradle plugin 加入了 [Transform API](http://tools.android.com/tech-docs/new-build-system/transform-api)，允许第三方插件在 class 文件转换成 dex 文件之前来操控 class 文件，这样就很容易在不入侵代码的情况下实现一些 AOP 的操作。

![](build.png)

![](screenshot.png)

这里 Android 官方也实现了很多 transform，这里有一个`DesugarTransform`，是 Android 给使用了 java8 的代码做脱糖处理的，所以说在 Android 上面使用 java8，其实只是语法糖而已。

![](transform.png)

下图给出了 transform 的工作方式，即上一个 transform 的输出作为下一个 transform 的输入。既然这样，那么肯定有先后顺序的问题，比如我们自定义了一个 `CustomTransfrom`，作用是修改 `com.panda912.asmdemo.Test ` 类中的某些东西，但是官方又实现了一个 `ProGuardTransform` 用来处理代码混淆，如果是先执行 `ProGuardTransform` ，后执行 `CustomTransfrom` ，那么代码先被混淆了，包名类名都变了， `CustomTransfrom` 肯定不生效了。那么怎么控制这些 transform 的执行顺序呢？答案在 `com.android.build.gradle.internal.TaskManager#createPostCompilationTasks(VariantScope)`，代码太长就不贴了，他会在编译完成之后，准备生成 dex 文件之前，Desugar -> MergeJavaRes -> **apply all the external transforms** -> Android studio profiling transforms -> JavaCodeShrinker (Proguard / BuiltInShrinker)-> ResourcesShrinker -> …...

所以，我们大概率不用担心我们自定义的 transform 和 Android 官方提供的 transform 冲突，而在 apply all the external transforms 这一步，就是循环遍历项目中引用的第三方的自定义的 plugin 中的 transform ，先后执行顺序也就提现在 build.gradle 中 `apply plugin: xxx` 的先后顺序上。谁写在前面，谁就先执行。

本文将通过一个🌰来介绍自定义 gradle plugin 的流程。

需求：

1. 对字符串加密，例如把 `public String TAG = "明文";`转换成`public String TAG = Crypto.decode("密文");`
2. 支持在 build.gradle 中配置对指定包名下的所有类中的字符串常量进行加密。