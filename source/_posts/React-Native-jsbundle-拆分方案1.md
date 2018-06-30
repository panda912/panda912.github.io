---
title: React Native jsbundle 拆分方案1
date: 2017-08-21 20:35:36
tags: React Native
---

参考: 『携程是如何做 React Native 优化的』

####为什么拆分?

1. jsbundle 太大, 一个简单的页面, 打包之后的 jsbundle 都600多K, 拆分之后可以做热更新, 节省用户流量;
2. 拆分基础包和业务包, 基础包可以做预加载,解决冷启动 RN 页面白屏问题;

####怎么拆分?

既然要拆分 jsbundle, 那就看看 jsbundle 里都有啥:

![](1.png)

![](2.png)

 前面 `!function` 开头的代码是各个依赖模块引用的部分, 中间的 `__d(function(...){...},XXX)` 部分是各个入口模块和业务模块定义部分, 后面的数字是各个模块对应的 `moduleId`, 最后的 `require(XX)` 是入口模块的注册部分. 

如果你再打另外一个模块的 jsbundle 与之对比, 会发现代码大致相同, 区别最大的就是最后一行 `require(XX)` 中那个moduleId 所对应的`__d(function(…){…},XXX)`, 就是入口模块的定义部分.

所以我们新建一个 js 文件, 命名 `base.android.js`, 只导入这两个基础模块,

```react
import React from 'react';
import {} from 'react-native';
```

这个 js 文件所打出来的 jsbundle 就是基础包了, 不包含任何业务代码. 我们把基础包命名为 `base.android.jsbundle`, 把业务包命名为 `biz.android.jsbundle`, 两个文件做差分, 生成的 diff 文件就是增量包了, 正常发包的时候把基础包集成进 apk, 如果业务包有更新, 只需要更新增量包, 然后做patch合并, 合并之后的文件就是完整的 jsbundle 文件了.

可以使用 [bsdiff](http://www.daemonology.net/bsdiff/), 或者 [jbdiff](https://github.com/jdesbonnet/jbdiff) 生成 diff 文件以及补丁的合并,  一个 C 版本, 一个 Java 版本的.

如果要再减小包大小,就在压缩一下喽, 下载之后再解压缩, 然后合并文件.

####可是这并无法做预加载, 因为并不能直接加载基础包, 会报错.