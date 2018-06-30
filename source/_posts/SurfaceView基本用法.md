---
title: SurfaceView基本用法
date: 2016-10-18 14:19:48
tags: 自定义 View
---

`SurfaceView`一半多用于游戏开发，或者类似大转盘，时钟控件等动态实时更新的View，因为它要求在子线程中绘制，而不会阻塞主线程。



自定义View继承`SurfaceView`并且实现`SurfaceHolder.Callback`接口：

```java
/**
 * A client may implement this interface to receive information about
 * changes to the surface.  When used with a {@link SurfaceView}, the
 * Surface being held is only available between calls to
 * {@link #surfaceCreated(SurfaceHolder)} and
 * {@link #surfaceDestroyed(SurfaceHolder)}.  The Callback is set with
 * {@link SurfaceHolder#addCallback SurfaceHolder.addCallback} method.
 */
public interface Callback {
    /**
     * This is called immediately after the surface is first created.
     * Implementations of this should start up whatever rendering code
     * they desire.  Note that only one thread can ever draw into
     * a {@link Surface}, so you should not draw into the Surface here
     * if your normal rendering will be in another thread.
     * 
     * @param holder The SurfaceHolder whose surface is being created.
     */
    public void surfaceCreated(SurfaceHolder holder);

    /**
     * This is called immediately after any structural changes (format or
     * size) have been made to the surface.  You should at this point update
     * the imagery in the surface.  This method is always called at least
     * once, after {@link #surfaceCreated}.
     * 
     * @param holder The SurfaceHolder whose surface has changed.
     * @param format The new PixelFormat of the surface.
     * @param width The new width of the surface.
     * @param height The new height of the surface.
     */
    public void surfaceChanged(SurfaceHolder holder, int format, int width,
            int height);

    /**
     * This is called immediately before a surface is being destroyed. After
     * returning from this call, you should no longer try to access this
     * surface.  If you have a rendering thread that directly accesses
     * the surface, you must ensure that thread is no longer touching the 
     * Surface before returning from this function.
     * 
     * @param holder The SurfaceHolder whose surface is being destroyed.
     */
    public void surfaceDestroyed(SurfaceHolder holder);
}
```

`surfaceCreated`在surface第一次创建的时候会调用，一般在该方法中创建并启动绘制线程；

`surfaceChanged`在surface发生变化时会调用；

`surfaceDestroyed`在surface被销毁的时候调用，一般在这里停止绘制线程。

> 它会在surface被销毁之前立即调用，当被调用之后，就不不应该再试图访问surface。如果你有一个渲染线程直接访问surface，你应该确保这个线程在该方法调用之前不再有关联。

我们可以通过`SurfaceView`中`getHolder()`来获取`SurfaceHolder`，再通过`SurfaceHolder`的`lockCanvas()`来获取Canvas，

```java
public Canvas lockCanvas(Rect dirty);
```

脏矩形，只刷新该区域，优化性能，注意双缓冲的问题。

获取完画布就可以做绘制的事情了，绘制完成之后调用要`SurfaceHolder`的`unlockCanvasAndPost(Canvas canvas)`提交画布。

时钟Demo：

```java
public class Clock extends SurfaceView implements SurfaceHolder.Callback {

    private final Object lock = new Object();

    private SurfaceHolder mSurfaceHolder;

    private DrawThread mDrawThread;
    /**
     * 面板
     */
    private Paint mPanelPaint;
    /**
     * 刻度
     */
    private Paint mScalePaint;
    /**
     * 数字
     */
    private Paint mTextPaint;
    /**
     * 时针
     */
    private Paint mHourPaint;
    /**
     * 分针
     */
    private Paint mMinutePaint;
    /**
     * 秒针
     */
    private Paint mSecondPaint;

    private int mRadius;

    public Clock(Context context) {
        this(context, null);
    }

    public Clock(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public Clock(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);

        mSurfaceHolder = this.getHolder();
        mSurfaceHolder.addCallback(this);

        //面板
        mPanelPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPanelPaint.setColor(Color.RED);

        //刻度
        mScalePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mScalePaint.setColor(Color.argb(130, 255, 255, 255));

        mTextPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mTextPaint.setColor(Color.WHITE);
        mTextPaint.setTextSize(40);
        mTextPaint.setTextAlign(Paint.Align.CENTER);

        //时针
        mHourPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mHourPaint.setColor(Color.WHITE);

        //分针
        mMinutePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mMinutePaint.setColor(Color.argb(200, 255, 255, 255));

        //秒针
        mSecondPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mSecondPaint.setStrokeWidth(2);
        mSecondPaint.setColor(Color.GRAY);

        setFocusable(false);
        setFocusableInTouchMode(false);
    }

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        mDrawThread = new DrawThread();
        mDrawThread.setRun(true);
        mDrawThread.start();
    }

    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {

    }

    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        mDrawThread.setRun(false);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        mRadius = Math.min(getMeasuredWidth(), getMeasuredHeight()) / 2;
    }

    private class DrawThread extends Thread {

        private static final float RATIO = 6 / 7f;

        private boolean isRun;

        @Override
        public void run() {
            while (isRun) {
                synchronized (lock) {
                    Canvas canvas = mSurfaceHolder.lockCanvas();
                    if (canvas != null) {
                        long start = System.currentTimeMillis();
                        doDraw(canvas);
                        mSurfaceHolder.unlockCanvasAndPost(canvas);
                        long end = System.currentTimeMillis();
                        long duration = end - start;
                        if (duration < 1000) {
                            try {
                                Thread.sleep(1000 - duration);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
            }
        }

        /**
         * 画面板
         *
         * @param canvas
         */
        private void doDraw(final Canvas canvas) {
            //清屏
            canvas.drawColor(Color.RED);
            //面板
            canvas.drawCircle(mRadius, mRadius, mRadius, mPanelPaint);
            //刻度
            for (int i = 1, scaleSize = 12 * 5 * 5; i <= scaleSize; i++) {
                canvas.rotate(1.2f, mRadius, mRadius);
                if (i % 25 == 0) {
                    mScalePaint.setStrokeWidth(4);
                    mScalePaint.setColor(Color.argb(150, 255, 255, 255));
                    canvas.drawLine(mRadius, 0, mRadius, 50, mScalePaint);
                } else if (i % 5 == 0) {
                    mScalePaint.setStrokeWidth(3);
                    mScalePaint.setColor(Color.argb(120, 255, 255, 255));
                    canvas.drawLine(mRadius, 0, mRadius, 30, mScalePaint);
                } else {
                    mScalePaint.setStrokeWidth(2);
                    mScalePaint.setColor(Color.argb(100, 255, 255, 255));
                    canvas.drawLine(mRadius, 0, mRadius, 18, mScalePaint);
                }
            }

            RectF f = new RectF();


            Calendar calendar = Calendar.getInstance();
            int hour = calendar.get(Calendar.HOUR_OF_DAY);
            int minute = calendar.get(Calendar.MINUTE);
            int second = calendar.get(Calendar.SECOND);

            canvas.save();
            canvas.rotate(30 * (hour + (minute + second / 60) / 60f), mRadius, mRadius);
            canvas.drawRoundRect(new RectF(mRadius - 5, mRadius / 3, mRadius + 5, mRadius + 20), 10, 10, mHourPaint);
            canvas.restore();

            canvas.save();
            canvas.rotate(6 * (minute + second / 60f), mRadius, mRadius);
            canvas.drawRoundRect(new RectF(mRadius - 3f, 60, mRadius + 3f, mRadius + 20), 10, 10, mMinutePaint);
            canvas.restore();

            canvas.save();
            canvas.rotate(6 * second, mRadius, mRadius);
            mSecondPaint.setStyle(Paint.Style.FILL);
            canvas.drawRect(new RectF(mRadius - 2, 50, mRadius + 2, mRadius + 30), mSecondPaint);
            canvas.restore();


            for (int i = 1; i <= 12; i++) {

                float width = mTextPaint.measureText(String.valueOf(i));

                double x = mRadius + (mRadius - 50) * Math.cos((i * 30 - 90) * Math.PI / 180d);
                double y = mRadius + (mRadius - 50) * Math.sin((i * 30 - 90) * Math.PI / 180d) - width / 2 * Math.sin(i * 30);
                canvas.drawText(String.valueOf(i), (float) x, (float) y, mTextPaint);
            }

            //////////////////////////////////////////////////////////////////////////////////
            //x1 = x0 + r * cos(angle * pi / 180)
            //y1 = y0 + r * sin(angle * pi / 180)
//            double x = mRadius + (mRadius - 60) * Math.cos((second * 6 - 90) * Math.PI / 180d);
//            double y = mRadius + (mRadius - 60) * Math.sin((second * 6 - 90) * Math.PI / 180d);
//
//            double tailX = x - (x - mRadius) / RATIO;
//            double tailY = y - (y - mRadius) / RATIO;
//
//            Path path = new Path();
//            mSecondPaint.setStyle(Paint.Style.STROKE);
//            path.moveTo((float) tailX, (float) tailY);
//            path.lineTo((float) x, (float) y);
//            canvas.drawPath(path, mSecondPaint);


            //中心圆点
            mSecondPaint.setStyle(Paint.Style.FILL);
            canvas.drawCircle(mRadius, mRadius, 6, mSecondPaint);
        }

        void setRun(boolean isRun) {
            this.isRun = isRun;
        }
    }

    public void setRun(boolean isRun) {
        mDrawThread.setRun(isRun);
    }

    public void setRadius(int radius) {
        this.mRadius = radius;
    }
}
```