---
title: AndroidManifest 知多少
date: 2016-08-25 07:05:45
tags:
---

#### Activity

##### 1. noHistory

> Specify whether an activity should be kept in its history stack. If this attribute is set, then as soon as the user navigates away from the activity it will be finished and they will no longer be able to return to it.

指定一个 Activity 是否应该保留在其所在的历史栈中。如果指定了这个属性，那么用户一旦导航到其他的 Activity ，这个 Activity就会被 finish 掉，并且返回的时候也不会返回到该 Activity。

最常见的用法就是从 Splash 页跳转到 Home 页。

##### 2. excludeFromRecents

> Indicates that an Activity should be excluded from the list of recently launched activities.

如果指定了这个属性，则表明这个 Activity 应该被排除在最近启动的 Activity 列表中。

如果你想用 Activity 做一些‘奇怪’的事情且不想让用户看到，例如使用一个透明的 Activity 做页面统跳处理，可以把这个 Activity 设置为透明主题，并指定这个属性，那么在任务卡片中就不会看到了。

##### 3. taskAffinity

> Specify a task name that activities have an "affinity" to. Use with the application tag (to supply a default affinity for all activities in the application), or with the activity tag (to supply a specific affinity for that component).
>
> The default value for this attribute is the same as the package name, indicating that all activities in the manifest should generally be considered a single "application" to the user.  You can use this attribute to modify that behavior: either giving them an affinity for another task, if the activities are intended to be part of that task from the user's perspective, or using an empty string for activities that have no affinity to a task.

简单地说，就是给 Activity 指定一个所依附的任务栈。通常来讲，同一个 App 中的 Activity 默认都是依附在同一个任务栈中的，但是我们也可以给某个 Activity 特殊指定其所依附的任务栈。