---
title: 仿同程旅游app产品详情页，实现scrollview中tab顶部悬停效果
date: 2016-10-14 16:06:49
tags: 自定义 View
---

### #效果图
<table><tr><td><div align=center>
![pic1](https://ws1.sinaimg.cn/large/ad5b14bfgy1fcq007knzpg207i0dcb2b)
</div></td>
<td><div align=center>
![pic2](https://ws1.sinaimg.cn/large/ad5b14bfgy1fcq0cjcbibg207i0dce83)
</div></td></tr></table>

### #实现方式

1.在顶部放一个与scrollview中相同布局的浮动view，控制显示/隐藏；

2.view.setTranslationY(translationY)；

如果这个浮动的view有多种状态（类似tab切换这种），那么方式1就比较麻烦，要同步状态，所以本文是用方式2实现的。

### #代码分析

自定义属性，可以对照下图理解：

```xml
<declare-styleable name="FloatTabScrollView">
    <attr name="top_layout" format="reference" />
    <attr name="page1_layout" format="reference" />
    <attr name="page2_layout" format="reference" />
    <attr name="page3_layout" format="reference" />
    <attr name="bottom_layout" format="reference" />
    <attr name="tab1_title" format="string" />
    <attr name="tab2_title" format="string" />
    <attr name="tab3_title" format="string" />
    <attr name="selected_tab_color" format="color" />
    <attr name="unselected_tab_color" format="color" />
</declare-styleable>
```

<div align=center>

![pic3](https://ws2.sinaimg.cn/large/ad5b14bfgw1f8v0v232o4j20850g1glp.jpg)

</div>

在自定义的`FloatTabScrollView`的构造方法中`inflate`一个布局文件和获取自定义属性:

```java
public FloatTabScrollView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    mContext = context;
    inflate(context, R.layout.layout_float_tab_scrollview, this);

    TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.FloatTabScrollView, 0, 0);
    if (ta != null) {
        if (ta.hasValue(R.styleable.FloatTabScrollView_tab1_title)) {
            mTabTitle1 = ta.getString(R.styleable.FloatTabScrollView_tab1_title);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_tab2_title)) {
            mTabTitle2 = ta.getString(R.styleable.FloatTabScrollView_tab2_title);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_tab3_title)) {
            mTabTitle3 = ta.getString(R.styleable.FloatTabScrollView_tab3_title);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_top_layout)) {
            mTopLayoutId = ta.getResourceId(R.styleable.FloatTabScrollView_top_layout, 0);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_page1_layout)) {
            mPage1LayoutId = ta.getResourceId(R.styleable.FloatTabScrollView_page1_layout, 0);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_page2_layout)) {
            mPage2LayoutId = ta.getResourceId(R.styleable.FloatTabScrollView_page2_layout, 0);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_page3_layout)) {
            mPage3LayoutId = ta.getResourceId(R.styleable.FloatTabScrollView_page3_layout, 0);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_bottom_layout)) {
            mBottomLayoutId = ta.getResourceId(R.styleable.FloatTabScrollView_bottom_layout, 0);
        }
        if (ta.hasValue(R.styleable.FloatTabScrollView_tabline_color)) {
            tabLineColor = ta.getColor(R.styleable.FloatTabScrollView_tabline_color, 0xFFFF4081);
        }

        ta.recycle();
    }

}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.NestedScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nestedscrollview"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:overScrollMode="never">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <FrameLayout
                android:id="@+id/fl_top"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

            <!--给tab占位的view-->
            <View
                android:id="@+id/view_tab_holder"
                android:layout_width="match_parent"
                android:layout_height="45dp" />

            <FrameLayout
                android:id="@+id/fl_page1"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

            <FrameLayout
                android:id="@+id/fl_page2"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

            <FrameLayout
                android:id="@+id/fl_page3"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

            <FrameLayout
                android:id="@+id/fl_bottom"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />

        </LinearLayout>

        <!--tab-->
        <LinearLayout
            android:id="@+id/ll_tab"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@android:color/white"
            android:orientation="vertical">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="43dp"
                android:orientation="horizontal">

                <TextView
                    android:id="@+id/tv_tab1"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1"
                    android:gravity="center"
                    tools:text="商品详情" />

                <TextView
                    android:id="@+id/tv_tab2"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1"
                    android:gravity="center"
                    tools:text="商品参数" />

                <TextView
                    android:id="@+id/tv_tab3"
                    android:layout_width="0dp"
                    android:layout_height="match_parent"
                    android:layout_weight="1"
                    android:gravity="center"
                    tools:text="商品推荐" />

            </LinearLayout>

            <View
                android:id="@+id/view_tab_line"
                android:layout_width="120dp"
                android:layout_height="2dp"
                android:background="#FFFF4081" />

        </LinearLayout>

    </FrameLayout>

</android.support.v4.widget.NestedScrollView>
```

`ScrollView`下的子布局必须是`FrameLayout`或者`RelativeLayout`，在`tab`的位置放一个占位的`View`，如果占位view没有到达顶部，则让tab一直显示在占位view的位置，如果占位view到达了顶部，则让tab“固定”在顶部，为什么“固定”要加引号，其实并不是固定，tab一直在`ScrollView`中滚动，只是相对静止而已。

然后在onFinishInflate中加载每一块区域的布局文件：

```java
@Override
protected void onFinishInflate() {
    super.onFinishInflate();

    mTopContainerFL = (FrameLayout) findViewById(R.id.fl_top);
    mPage1ContainerFL = (FrameLayout) findViewById(R.id.fl_page1);
    mPage2ContainerFL = (FrameLayout) findViewById(R.id.fl_page2);
    mPage3ContainerFL = (FrameLayout) findViewById(R.id.fl_page3);
    mBottomContainerFL = (FrameLayout) findViewById(R.id.fl_bottom);

    inflate(mContext, mTopLayoutId, mTopContainerFL);
    inflate(mContext, mPage1LayoutId, mPage1ContainerFL);
    inflate(mContext, mPage2LayoutId, mPage2ContainerFL);
    inflate(mContext, mPage3LayoutId, mPage3ContainerFL);
    inflate(mContext, mBottomLayoutId, mBottomContainerFL);

    mTabHolderView = findViewById(R.id.view_tab_holder);
    mTabLL = (LinearLayout) findViewById(R.id.ll_tab);
    mTabLine = findViewById(R.id.view_tab_line);
    mTabLine.setBackgroundColor(tabLineColor);

    mTabTV1 = (TextView) findViewById(R.id.tv_tab1);
    mTabTV2 = (TextView) findViewById(R.id.tv_tab2);
    mTabTV3 = (TextView) findViewById(R.id.tv_tab3);
    mTabTV1.setText(mTabTitle1);
    mTabTV2.setText(mTabTitle2);
    mTabTV3.setText(mTabTitle3);

    mTabTV1.setOnClickListener(this);
    mTabTV2.setOnClickListener(this);
    mTabTV3.setOnClickListener(this);

    getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
        @Override
        public void onGlobalLayout() {
            setTabViewPosition(0);
            if (bGradient) {
                mToolbar.getBackground().setAlpha(MIN_ALPHA);
            }
            getViewTreeObserver().removeGlobalOnLayoutListener(this);
        }
    });
}
```
覆写`onScrollChanged`方法来平移tab的位置：

```java
@Override
protected void onScrollChanged(int scrollX, int scrollY, int oldScrollX, int oldScrollY) {
    super.onScrollChanged(scrollX, scrollY, oldScrollX, oldScrollY);

    setTabViewPosition(scrollY);

    if (bGradient) {
        float alpha = scrollY / (float) (mTabHolderView.getTop() - mToolbar.getHeight()) * MAX_ALPHA;
        mToolbar.getBackground().setAlpha(Math.min((int) alpha, MAX_ALPHA));
    }
}
```

```java
private void setTabViewPosition(int scrollY) {

    if (scrollY <= mPage3ContainerFL.getTop() + mPage3ContainerFL.getHeight() - mTabLL.getHeight()) {
        //设置tabview在scrollview中的位置
        mTabLL.setTranslationY(Math.max(scrollY + (mToolbar != null ? mToolbar.getHeight() : 0), mTabHolderView.getTop()));

        //滚动过程中切换tab的选中状态
        if (scrollY < mPage2ContainerFL.getTop() - mTabLL.getHeight() - (mToolbar != null ? mToolbar.getHeight() : 0)) {
            selectTab(0);
        } else if (scrollY >= mPage2ContainerFL.getTop() - mTabLL.getHeight() - (mToolbar != null ? mToolbar.getHeight() : 0)
                && scrollY < mPage3ContainerFL.getTop() - mTabLL.getHeight() - (mToolbar != null ? mToolbar.getHeight() : 0)) {
            selectTab(1);
        } else if (scrollY >= mPage3ContainerFL.getTop() - mTabLL.getHeight() - (mToolbar != null ? mToolbar.getHeight() : 0)) {
            selectTab(2);
        }
    } else {
        //超过最大高度之后,把tabview设置在原始位置,,,,,,也可以不设置
        mTabLL.setTranslationY(mTabHolderView.getTop());
    }
}
```

```java
private void selectTab(int index) {

    if (mTabLineAnimator == null) {
        mTabLineAnimator = new ValueAnimator();
        mTabLineAnimator.setDuration(DEFAULT_DURATION);
        mTabLineAnimator.setInterpolator(new FastOutSlowInInterpolator());
        mTabLineAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mTabLine.setTranslationX((int) animation.getAnimatedValue());
            }
        });
    }

    int curTabLineX = (int) mTabLine.getX();
    int needTabLineX = 0;

    switch (index) {
        case 0:
            needTabLineX = 0;
            break;
        case 1:
            needTabLineX = mTabLL.getWidth() / 3;
            break;
        case 2:
            needTabLineX = mTabLL.getWidth() / 3 * 2;
            break;
        default:
            break;
    }

    if (curTabLineX == needTabLineX) {
        return;
    }
    mTabLineAnimator.setIntValues(curTabLineX, needTabLineX);
    if (!mTabLineAnimator.isRunning()) {
        mTabLineAnimator.start();
    }
}
```

### #项目地址

[https://github.com/panda912/FloatTabScrollView](https://github.com/panda912/FloatTabScrollView)