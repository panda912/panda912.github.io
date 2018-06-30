---
title: library中BuildConfig.DEBUG始终为false的解决方法
date: 2016-09-02 10:27:39
tags: gradle
---

#### 参考：

> http://stackoverflow.com/questions/20176284/buildconfig-debug-always-false-when-building-library-projects-with-gradle#

#### 修改默认的发布配置

通过 android.publishNonDefault = true 启用其他variants的发布

通过 compile project(path: ':project', configuration: 'flavor1Debug') 引用指定的variant

> library module :

```groovy
android {
	publishNonDefault true
}
```
> app module:

```groovy
dependencies {
	debugCompile project(path: ':library', configuration: 'debug')
	releaseCompile project(path: ':library', configuration: 'release')
}
```
#### 自定义buildconfig field

> library module:

```groovy
android {
	buildTypes {
    	debug {
        	buildConfigField "boolean", "LOG_DEBUG", "true"
    	}
    	release {
        	buildConfigField "boolean", "LOG_DEBUG", "false"
    	}
	}
}
```
> java code

```java
if (BuildConfig.LOG_DEBUG) {
	// TODO
}
```
ReBuild之后你会发现debug和release模式下`BuildConfig`中`LOG_DEBUG`的值是不同的。

这种方式不仅用与此，你还可以添加其他的字段，比如debug和release模式下不同的API base url，等等。