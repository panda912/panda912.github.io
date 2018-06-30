---
title: Android Drawable 之 LevelListDrawable
date: 2016-12-23 12:02:28
tags: Drawable
---

本文演示使用`LevelListDrawable`结合`android-Ultra-Pull-To-Refresh`实现同程旅游App 的首页下拉刷新效果，Demo使用到的图片资源是解压缩同程App 取到的。

![](https://ws2.sinaimg.cn/large/ad5b14bfgw1fb8lvz5dddg207i0dcqv5.gif)

### 效果分析：

仔细观察这个下拉效果不难发现，下拉的时候其实就是根据下拉的距离显示不同的图片，刷新的时候就是显示一个loading 动画。

### 代码目录：

![pic1](https://ws2.sinaimg.cn/large/ad5b14bfgw1fb8ktdt6vdj205i026t8o.jpg)

![pic2](https://ws4.sinaimg.cn/large/ad5b14bfgw1fb8ktdtmadj204d01m0sm.jpg)

### 代码片段：

#### tc_pull.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<level-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@drawable/loading_home_1" android:maxLevel="0" />
    <item android:drawable="@drawable/loading_home_2" android:maxLevel="1" />
    <item android:drawable="@drawable/loading_home_3" android:maxLevel="2" />
    <item android:drawable="@drawable/loading_home_4" android:maxLevel="3" />
    <item android:drawable="@drawable/loading_home_5" android:maxLevel="4" />
    <item android:drawable="@drawable/loading_home_6" android:maxLevel="5" />
    <item android:drawable="@drawable/loading_home_7" android:maxLevel="6" />
    <item android:drawable="@drawable/loading_home_8" android:maxLevel="7" />
    <item android:drawable="@drawable/loading_home_9" android:maxLevel="8" />
    <item android:drawable="@drawable/loading_home_10" android:maxLevel="9" />
    <item android:drawable="@drawable/loading_home_11" android:maxLevel="10" />
    <item android:drawable="@drawable/loading_home_12" android:maxLevel="11" />
    <item android:drawable="@drawable/loading_home_13" android:maxLevel="12" />
    <item android:drawable="@drawable/loading_home_14" android:maxLevel="13" />
    <item android:drawable="@drawable/loading_home_15" android:maxLevel="14" />
    <item android:drawable="@drawable/loading_home_16" android:maxLevel="15" />
    <item android:drawable="@drawable/loading_home_17" android:maxLevel="16" />
    <item android:drawable="@drawable/loading_home_18" android:maxLevel="17" />
    <item android:drawable="@drawable/loading_home_19" android:maxLevel="18" />
    <item android:drawable="@drawable/loading_home_20" android:maxLevel="19" />
    <item android:drawable="@drawable/loading_home_21" android:maxLevel="20" />
    <item android:drawable="@drawable/loading_home_22" android:maxLevel="21" />
    <item android:drawable="@drawable/loading_home_23" android:maxLevel="22" />
</level-list>
```

#### tc_loading.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false">
    <item android:drawable="@drawable/loop_home_01" android:duration="70" />
    <item android:drawable="@drawable/loop_home_01" android:duration="70" />
    <item android:drawable="@drawable/loop_home_02" android:duration="70" />
    <item android:drawable="@drawable/loop_home_03" android:duration="70" />
    <item android:drawable="@drawable/loop_home_04" android:duration="70" />
    <item android:drawable="@drawable/loop_home_05" android:duration="70" />
    <item android:drawable="@drawable/loop_home_06" android:duration="70" />
    <item android:drawable="@drawable/loop_home_07" android:duration="70" />
    <item android:drawable="@drawable/loop_home_08" android:duration="70" />
    <item android:drawable="@drawable/loop_home_09" android:duration="70" />
    <item android:drawable="@drawable/loop_home_10" android:duration="70" />
    <item android:drawable="@drawable/loop_home_11" android:duration="70" />
    <item android:drawable="@drawable/loop_home_12" android:duration="70" />
    <item android:drawable="@drawable/loop_home_13" android:duration="70" />
    <item android:drawable="@drawable/loop_home_14" android:duration="70" />
    <item android:drawable="@drawable/loop_home_15" android:duration="70" />
    <item android:drawable="@drawable/loop_home_16" android:duration="70" />
    <item android:drawable="@drawable/loop_home_17" android:duration="70" />
</animation-list>
```

#### TCPtrHeader.java

```java
public class TCPtrHeader extends FrameLayout implements PtrUIHandler {
    public static final String TAG = TCPtrHeader.class.getSimpleName();

    private ImageView mPullIV;

    public TCPtrHeader(Context context) {
        this(context, null);
    }

    public TCPtrHeader(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public TCPtrHeader(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init() {
        mPullIV = new ImageView(getContext());
        mPullIV.setPadding(0, 30, 0, 0);
        mPullIV.setLayoutParams(new LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT));
        mPullIV.setImageResource(R.drawable.tc_pull);
        mPullIV.setScaleType(ImageView.ScaleType.FIT_CENTER);

        addView(mPullIV);
    }

    @Override
    public void onUIReset(PtrFrameLayout frame) {
    }

    @Override
    public void onUIRefreshPrepare(PtrFrameLayout frame) {
    }

    @Override
    public void onUIRefreshBegin(PtrFrameLayout frame) {
        mPullIV.setImageResource(R.drawable.tc_loading);
        AnimationDrawable drawable = (AnimationDrawable) mPullIV.getDrawable();
        drawable.start();
    }

    @Override
    public void onUIRefreshComplete(PtrFrameLayout frame) {
        mPullIV.setImageResource(R.drawable.tc_pull);
    }

    @Override
    public void onUIPositionChange(PtrFrameLayout frame, boolean isUnderTouch, byte status, PtrIndicator ptrIndicator) {
        final int offsetToRefresh = frame.getOffsetToRefresh();
        final int currentPos = ptrIndicator.getCurrentPosY();
        final int imgLevel = currentPos / (offsetToRefresh / 23);

        mPullIV.setImageLevel(Math.min(imgLevel, 22));
    }
}
```
### 总结：

本文主要是介绍`LevelListDrawable`的基本用法，说实话我之前是不知道有这个东西的，其实 Android 给我们提供了许多API，我们要善于利用。