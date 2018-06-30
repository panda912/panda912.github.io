---
title: Android L 使用 ViewOutlineProvider 裁剪 View
date: 2017-01-12 10:53:58
tags: Android L新特性
---

Android 5.0的 `View` 类中新增了 `setOutlineProvider(ViewOutlineProvider provider)`  方法，注释如下：

> Sets the {@link ViewOutlineProvider} of the view, which generates the Outline that defines the shape of the shadow it casts, and enables outline clipping.
>
> The default ViewOutlineProvider, {@link ViewOutlineProvider#BACKGROUND}, queries the Outline from the View's background drawable, via {@link Drawable#getOutline(Outline)}. Changing the outline provider with this method allows this behavior to be overridden.
>
> If the ViewOutlineProvider is null, if querying it for an outline returns false, or if the produced Outline is {@link Outline#isEmpty()}, shadows will not be cast.
>
> Only outlines that return true from {@link Outline#canClip()} may be used for clipping.
>
> @see #setClipToOutline(boolean)
>
> @see #getClipToOutline()
>
> @see #getOutlineProvider() 

那么我们可以用它来把 `View` 裁剪成一些特定(圆形、矩形、圆角矩形)的形状：

```java
view.setOutlineProvider(new ViewOutlineProvider() {
    @Override
    public void getOutline(View view, Outline outline) {
        outline.setRoundRect(0, 0, view.getWidth(), view.getHeight(), 30);
    }
});
view.setClipToOutline(true);
```

![](https://ws4.sinaimg.cn/mw690/ad5b14bfgw1fbouzvhy1lj21091n676p.jpg)

也可以用来设置投影，但是投影的形状只能是凸多边形，为什么？看源码：

```java
/**
 * Sets the Constructs an Outline from a
 * {@link android.graphics.Path#isConvex() convex path}.
 */
public void setConvexPath(@NonNull Path convexPath) {
    if (convexPath.isEmpty()) {
        setEmpty();
        return;
    }
    //如果不是凸多边形，会抛异常
    if (!convexPath.isConvex()) {
        throw new IllegalArgumentException("path must be convex");
    }
    mMode = MODE_CONVEX_PATH;
    mPath.set(convexPath);
    mRect.setEmpty();
    mRadius = RADIUS_UNDEFINED;
}
```

```java
view.setElevation(5);
view.setOutlineProvider(new ViewOutlineProvider() {
    @Override
    public void getOutline(View view, Outline outline) {
      	//你可以用 Path 指定任何的形状，前提是凸多边形
        //这里设置投影的位置从右下角开始，投影形状是矩形
        Path path = new Path();
        path.moveTo(view.getWidth(), view.getHeight());
        path.lineTo(view.getWidth(), view.getHeight() * 2);
        path.lineTo(view.getWidth() * 2, view.getHeight() * 2);
        path.lineTo(view.getWidth() * 2, view.getHeight());
        path.close();
        outline.setConvexPath(path);
    }
});
```

![](https://ws4.sinaimg.cn/mw690/ad5b14bfgw1fbouzvk4bwj21091n6wgz.jpg)