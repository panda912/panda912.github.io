---
title: Scroller 基本用法
date: 2017-01-20 15:58:19
tags: 自定义 View
---

`Scroller`是什么：

`Scroller`是一个用来辅助`View`滚动的工具类。它本身并不能够控制 View 的滚动，它只是用来辅助滚动的。

小明：什么意思，我自己就可以让 View 滚动啊，为什么还要它来辅助我？

老师问小明：在一个 ViewGroup 中把 A 点的一个 View 拉到 B 点，**松手之后**怎么让这个 View 再回到 A 点呢？奥，不对，是平滑的滚动到 A 点。

小明想了想：可以使用动画！

老师：除了动画还有什么办法呢？

小明：不知道了。

老师：（嘻嘻）可以用 Scroller。

这里为什么强调松手之后，这就可以解释 Scroller 是用来辅助 View 做滚动，而不是使用它来让 View 滚动。比方说我们触摸一个 View，松手之前，我们可以在`onTouchEvent`里用`scrollTo`或`scrollBy`来控制 View 的滚动，当松手之后就可以使用 Scroller 的 `startScroll`方法，设置 View 的X轴和Y轴方向上的偏移坐标（这个偏移坐标以 View 的左上方在父控件的坐标系中为准）以及对应方向上想要滚动的距离。

PS：`scrollTo`和`scrollBy`都可以滚动 View 中的内容！View 中的内容！View 中的内容！不是滚动 View 自身。但是两者又有区别，看源码：

```java
/**
 * Set the scrolled position of your view. This will cause a call to
 * {@link #onScrollChanged(int, int, int, int)} and the view will be
 * invalidated.
 * @param x the x position to scroll to
 * @param y the y position to scroll to
 */
public void scrollTo(int x, int y) {
    if (mScrollX != x || mScrollY != y) {
        int oldX = mScrollX;
        int oldY = mScrollY;
        mScrollX = x;
        mScrollY = y;
        invalidateParentCaches();
        onScrollChanged(mScrollX, mScrollY, oldX, oldY);
        if (!awakenScrollBars()) {
            postInvalidateOnAnimation();
        }
    }
}
```

```java
/**
 * Move the scrolled position of your view. This will cause a call to
 * {@link #onScrollChanged(int, int, int, int)} and the view will be
 * invalidated.
 * @param x the amount of pixels to scroll by horizontally
 * @param y the amount of pixels to scroll by vertically
 */
public void scrollBy(int x, int y) {
    scrollTo(mScrollX + x, mScrollY + y);
}
```

scrollTo 中直接将传入的新的X轴和Y轴的偏移坐标与当前的偏移坐标比较，如果不同，则直接赋值给了`mScrollX`和`mScrollY`，所以调用多次`scrollTo`只会发生一次位置的变化，而调用多次 `scrollBy` 则会改变多次，每一次以上一次的偏移坐标为基准。

小明：老师你bb半天，到底怎么样能滚动啊？

老师：自定义 ViewGroup 重写`computeScroll`方法，在`computeScroll`中实现滚动。`computeScroll`是 View 类中的方法，这个方法是空的，并没有任何实现。这个方法是干什么的呢？字面意思就是计算滚动。我们看看源码：

```java
/**
 * Called by a parent to request that a child update its values for mScrollX
 * and mScrollY if necessary. This will typically be done if the child is
 * animating a scroll using a {@link android.widget.Scroller Scroller}
 * object.
 */
public void computeScroll() {
}
```

注释意思大概是：如果需要的话，父控件会调用这个方法来请求子控件更新mScrollX和mScrollY的值，如果子控件是使用 Scroller 来执行动画滚动，通常会这样做。

那我们就可以在这个方法里这样写：

```java
@Override
public void computeScroll() {
    if (mScroller.computeScrollOffset()) {
        scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
        invalidate();
    }
}
```

计算滚动偏移量，获取最新的偏移坐标并设置到`scrollTo`中，以此实现滚动效果。

```java
/**
 * Call this when you want to know the new location.  If it returns true,
 * the animation is not yet finished.
 */ 
public boolean computeScrollOffset() {
    if (mFinished) {
        return false;
    }
	...更新mCurrX和mCurrY的值...
    return true;
}
```

`computeScrollOffset`：当你想获取新的坐标点，你就可以调用它更新坐标点，然后调用`getCurrX`或`getCurrY` 来获取最新的坐标点。如果返回`true`表示这个滚动动画还没有完成。

用法比较简单，下面使用 Scroller 实现一个 Demo：

![](screen.gif)

```java
public class PullDownViewGroup extends ViewGroup {
    private static final String TAG = PullDownViewGroup.class.getSimpleName();

    /**
     * 阻尼
     */
    private static final float DAMPING = 1.5f;

    private View mContentView;
    private View mHeaderView;

    private Scroller mScroller;
    private int mTouchSlop;
    /**
     * 第一次触摸时的坐标
     */
    private float mDownY;
    /**
     * 上一次触发 ACTION_MOVE 时的坐标
     */
    private float mLastMoveY;
    /**
     * 当前坐标
     */
    private float mCurDownY;
    /**
     * 当前坐标和上一次 move 时的距离
     */
    private float mDeltaY;

    private boolean isShowHeader = false;

    public PullDownViewGroup(Context context) {
        this(context, null);
    }

    public PullDownViewGroup(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public PullDownViewGroup(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);

        mTouchSlop = ViewConfiguration.get(getContext()).getScaledTouchSlop();
        mScroller = new Scroller(getContext());
    }


    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
        int childCount = getChildCount();
        if (childCount == 2) {
            mHeaderView = getChildAt(0);
            mContentView = getChildAt(1);
        } else {
            throw new InflateException("Must have two children.");
        }
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        measureChildren(widthMeasureSpec, heightMeasureSpec);
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        mHeaderView.layout(0, -mHeaderView.getMeasuredHeight(), mHeaderView.getMeasuredWidth(), 0);
        mContentView.layout(0, 0, mContentView.getMeasuredWidth(), mContentView.getMeasuredHeight());
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
                mDownY = ev.getRawY();
                break;
            case MotionEvent.ACTION_MOVE:
                mLastMoveY = ev.getRawY();
                int mRealTouchSlop = (int) Math.abs(mLastMoveY - mDownY);
                if (mRealTouchSlop > mTouchSlop) { //当滑动距离大于临界值时，拦截事件（不让事件传递到子控件上），事件传递到本 ViewGroup 的 onTouchEvent。
                    return true;
                }
                break;
        }
        return super.onInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                break;
            case MotionEvent.ACTION_MOVE:
                mCurDownY = event.getRawY();
                mDeltaY = mCurDownY - mLastMoveY;
                Log.e(TAG, "mCurDownY:" + mCurDownY + "     mLastMoveY:" + mLastMoveY + "     mDeltaY:" + mDeltaY + "     getScrollY:" + getScrollY());
                if ((!isShowHeader && mDeltaY < 0) || (isShowHeader && mDeltaY > 0)) {
                    return true;
                }

                scrollBy(0, (int) (-mDeltaY / DAMPING));
                mLastMoveY = mCurDownY;

                if (getScrollY() < 0 && mDeltaY > 0 && -getScrollY() >= 300 && !isShowHeader) { //下拉倒临界值，切换到 headerview
                    invalidate();
                    isShowHeader = true;
                    mScroller.forceFinished(true);
                    int dy = -mHeaderView.getMeasuredHeight() - getScrollY();
                    Log.d(TAG, "下拉dy: " + dy);
                    mScroller.startScroll(0, getScrollY(), 0, dy); //为什么要减掉getScrollY？因为已经下拉了getScrollY，所以要去掉这么多
                    return true;
                }

                if (getScrollY() < 0 && mDeltaY < 0 && mHeaderView.getMeasuredHeight() - Math.abs(getScrollY()) > 300 && isShowHeader) {
                    invalidate();
                    isShowHeader = false;
                    mScroller.forceFinished(true);
                    int dy = mContentView.getMeasuredHeight() + getScrollY();
                    Log.d(TAG, "上拉dy: " + dy);
                    mScroller.startScroll(0, getScrollY(), 0, -getScrollY());
                    return true;
                }

                break;
            case MotionEvent.ACTION_UP:
                if ((Math.abs(getScrollY()) >= 300 && !isShowHeader)
                        || (Math.abs(getScrollY()) <= mHeaderView.getMeasuredHeight() - 300 && isShowHeader)
                        || !mScroller.isFinished()
                        || mScroller.computeScrollOffset()) {
                    return true;
                }

                invalidate();
                if (isShowHeader && mHeaderView.getMeasuredHeight() - Math.abs(getScrollY()) < 300) {
                    mScroller.startScroll(0, getScrollY(), 0, -mHeaderView.getMeasuredHeight() - getScrollY());
                }

                if (!isShowHeader && Math.abs(getScrollY()) < 300) {
                    mScroller.startScroll(0, getScrollY(), 0, -getScrollY());
                }
                break;
        }
        return super.onTouchEvent(event);
    }

    @Override
    public void computeScroll() {
        if (mScroller.computeScrollOffset()) {
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            invalidate();
        }
    }
}
```
