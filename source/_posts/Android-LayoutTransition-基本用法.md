---
title: Android LayoutTransition 基本用法
date: 2017-01-22 15:54:39
tags: 动画
---

### 简介

`LayoutTransition`是在 API11 的时候引入的，用来设置当布局发生变化时的动画效果。有时候当我们addView、removeView 或者设置 View 的 GONE、VISIBLE 时，不想让布局的变化显得很突兀，想有一个过渡的效果，就在xml布局文件中设置`android:animateLayoutChanges="true"`属性，这个属性就是用到了`LayoutTransition`。我们来看看官方提供的默认的动画是什么样子的：

先从`ViewGroup`的构造方法中看起:

```java
public ViewGroup(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
    super(context, attrs, defStyleAttr, defStyleRes);
    initViewGroup();
    initFromAttributes(context, attrs, defStyleAttr, defStyleRes);
}
```

点进去`initFromAttributes`方法：

```java
private void initFromAttributes(
        Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
    final TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.ViewGroup, defStyleAttr,
            defStyleRes);

    final int N = a.getIndexCount();
    for (int i = 0; i < N; i++) {
        int attr = a.getIndex(i);
        switch (attr) {
            case ......
            case ......
            case R.styleable.ViewGroup_animateLayoutChanges:
                boolean animateLayoutChanges = a.getBoolean(attr, false);
                if (animateLayoutChanges) {
                    setLayoutTransition(new LayoutTransition());
                }
                break;
            case ......
            case ......
        }
    }
    a.recycle();
}
```

可以看到当我们设置`android:animateLayoutChanges="true"`的时候，其实就是调用了`setLayoutTransition(new LayoutTransition());`设置默认的动画效果。我们今天主要来看看`LayoutTransition`这个类：

```java
public LayoutTransition() {
    if (defaultChangeIn == null) {
        // "left" is just a placeholder; we'll put real properties/values in when needed
        PropertyValuesHolder pvhLeft = PropertyValuesHolder.ofInt("left", 0, 1);
        PropertyValuesHolder pvhTop = PropertyValuesHolder.ofInt("top", 0, 1);
        PropertyValuesHolder pvhRight = PropertyValuesHolder.ofInt("right", 0, 1);
        PropertyValuesHolder pvhBottom = PropertyValuesHolder.ofInt("bottom", 0, 1);
        PropertyValuesHolder pvhScrollX = PropertyValuesHolder.ofInt("scrollX", 0, 1);
        PropertyValuesHolder pvhScrollY = PropertyValuesHolder.ofInt("scrollY", 0, 1);
        defaultChangeIn = ObjectAnimator.ofPropertyValuesHolder((Object)null,
                pvhLeft, pvhTop, pvhRight, pvhBottom, pvhScrollX, pvhScrollY);
        defaultChangeIn.setDuration(DEFAULT_DURATION);
        defaultChangeIn.setStartDelay(mChangingAppearingDelay);
        defaultChangeIn.setInterpolator(mChangingAppearingInterpolator);
        defaultChangeOut = defaultChangeIn.clone();
        defaultChangeOut.setStartDelay(mChangingDisappearingDelay);
        defaultChangeOut.setInterpolator(mChangingDisappearingInterpolator);
        defaultChange = defaultChangeIn.clone();
        defaultChange.setStartDelay(mChangingDelay);
        defaultChange.setInterpolator(mChangingInterpolator);

        defaultFadeIn = ObjectAnimator.ofFloat(null, "alpha", 0f, 1f);
        defaultFadeIn.setDuration(DEFAULT_DURATION);
        defaultFadeIn.setStartDelay(mAppearingDelay);
        defaultFadeIn.setInterpolator(mAppearingInterpolator);
        defaultFadeOut = ObjectAnimator.ofFloat(null, "alpha", 1f, 0f);
        defaultFadeOut.setDuration(DEFAULT_DURATION);
        defaultFadeOut.setStartDelay(mDisappearingDelay);
        defaultFadeOut.setInterpolator(mDisappearingInterpolator);
    }
    mChangingAppearingAnim = defaultChangeIn;
    mChangingDisappearingAnim = defaultChangeOut;
    mChangingAnim = defaultChange;
    mAppearingAnim = defaultFadeIn;
    mDisappearingAnim = defaultFadeOut;
}
```

在`LayoutTransition`的构造方法中给我们实现好了默认的动画效果，总共包含4中动画效果，分别是：

1. `APPEARING`：`View`出现动画，当`View`在父容器中出现时候的动画；
2. `DISAPPEARING`：`View`消失动画，当`View`在父容器中消失时候的动画；
3. `CHANGE_APPEARING`：由于某些`View`出现在父容器中时，容器中其他的`View`而发生变化时的动画；
4. `CHANGE_DISAPPEARING`：由于某些`View`在父容器中消失时，容器中其他的`View`而发生变化时的动画。

我们可以调用`setAnimator`给`LayoutTransition`分别设置这些动画。

### 代码片段

```java
LayoutTransition layoutTransition = new LayoutTransition();
mContentLL.setLayoutTransition(layoutTransition);
//Appearing
PropertyValuesHolder pvhAppearingTranslation = PropertyValuesHolder.ofFloat("translationX", 800, 0);
PropertyValuesHolder pvhAppearingAlpha = PropertyValuesHolder.ofFloat("alpha", 0, 1);
PropertyValuesHolder pvhAppearingScaleX = PropertyValuesHolder.ofFloat("scaleX", 0, 1);
PropertyValuesHolder pvhAppearingScaleY = PropertyValuesHolder.ofFloat("scaleY", 0, 1);
PropertyValuesHolder pvhAppearingRotationX = PropertyValuesHolder.ofFloat("rotationX", 0, 360);
ObjectAnimator appearingAnimator = ObjectAnimator.ofPropertyValuesHolder((Object) null, pvhAppearingTranslation, pvhAppearingAlpha, pvhAppearingScaleX, pvhAppearingScaleY, pvhAppearingRotationX);
//Disappearing
PropertyValuesHolder pvhDisappearingTranslation = PropertyValuesHolder.ofFloat("translationX", 0, 800);
PropertyValuesHolder pvhDisappearingAlpha = PropertyValuesHolder.ofFloat("alpha", 1, 0);
PropertyValuesHolder pvhDisappearingScaleY = PropertyValuesHolder.ofFloat("scaleY", 1, 0);
ObjectAnimator disappearingAnimator = ObjectAnimator.ofPropertyValuesHolder((Object) null, pvhDisappearingTranslation, pvhDisappearingAlpha, pvhDisappearingScaleY);

layoutTransition.setAnimator(LayoutTransition.APPEARING, appearingAnimator);
layoutTransition.setInterpolator(LayoutTransition.APPEARING, new OvershootInterpolator());
layoutTransition.setDuration(LayoutTransition.APPEARING, 1200);
layoutTransition.setAnimator(LayoutTransition.DISAPPEARING, disappearingAnimator);
```

![](screen.gif)