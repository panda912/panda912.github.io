---
title: shape中设置padding无效解决方案
date: 2016-10-20 17:32:08
tags: xml
---

最近在写布局的时候，给`LinearLayout`添加分割线，并且想让分割线有上下边距，于是就自定义一个`shape`：

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <size android:height="@dimen/line_height" />
    <solid android:color="@color/main_line" />
    <padding
        android:bottom="10dp"
        android:top="10dp" />
</shape>
```

可是事与愿违，实际效果并没有上下边距。

解决方法：

把`shape`放在`layer-list`的`item`中，直接在`item`的节点下设置`top`和`bottom`：

```xml
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:bottom="10dp"
        android:top="10dp">
        <shape android:shape="rectangle">
            <size android:height="@dimen/line_height" />
            <solid android:color="@color/main_line" />
        </shape>
    </item>
</layer-list>
```