---
title: gradle使用exclude移除重复依赖
date: 2016-09-20 17:15:54
tags: gradle
---

### exclude

+--- com.android.support:design:24.2.1

|　　+--- com.android.support:support-v4:24.2.1 (*)

|　　+--- com.android.support:appcompat-v7:24.2.1 (*)

|　　\\ --- com.android.support:recyclerview-v7:24.2.1

|　　　　+--- com.android.support:support-annotations:24.2.1

|　　　　+--- com.android.support:support-compat:24.2.1 (*)

|　　　　\\ --- com.android.support:support-core-ui:24.2.1 (*)

上面是`com.android.support:design:24.2.1`的依赖关系，如果项目已经依赖了`com.android.support:support-v4:24.2.1`，那么就可以吧`design`包中的`support-v4`移除。

    compile('com.android.support:design:24.2.1') {
        exclude group: 'com.android.support', module: 'support-v4'
    }

`group`对应`com.android.support`， `module`对应`support-v4`，如果只写`group`，不写`module`，那么将会移除`design`包下所有与`group`对应的包。

### transitive

用于自动处理子依赖项。默认为 true，gradle 自动添加子依赖项；设置为 false，则需要手动添加每个依赖项。

