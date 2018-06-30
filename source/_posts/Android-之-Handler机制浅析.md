---
title: Android 之 Handler机制浅析
date: 2017-02-06 11:35:28
tags:
---

关于 `Handler` 的用法相信大家都应该知道，比如这样：

```java
public class HandlerActivity extends AppCompatActivity {
    private MyHandler mHandler;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_handler);

        mHandler = new MyHandler(this);
      
        mHandler.sendMessage(mHandler.obtainMessage(0, "hello"));
//        Message message = Message.obtain();
//        message.what = 0;
//        message.obj = "hello world";
//        mHandler.sendMessage(message);
    }

    private static class MyHandler extends Handler {
        WeakReference<Activity> mActivity;

        MyHandler(Activity activity) {
            mActivity = new WeakReference<>(activity);
        }

        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case 0:
                    Toast.makeText(mActivity.get(), (String) msg.obj, Toast.LENGTH_SHORT).show();
                    break;
                default:
                    break;
            }
        }
    }
}
```
我们从 `Handler` 的构造方法开始看起：

```java
/**
 * Use the {@link Looper} for the current thread with the specified callback interface
 * and set whether the handler should be asynchronous.
 *
 * Handlers are synchronous by default unless this constructor is used to make
 * one that is strictly asynchronous.
 *
 * Asynchronous messages represent interrupts or events that do not require global ordering
 * with respect to synchronous messages.  Asynchronous messages are not subject to
 * the synchronization barriers introduced by {@link MessageQueue#enqueueSyncBarrier(long)}.
 *
 * @param callback The callback interface in which to handle messages, or null.
 * @param async If true, the handler calls {@link Message#setAsynchronous(boolean)} for
 * each {@link Message} that is sent to it or {@link Runnable} that is posted to it.
 *
 * @hide
 */
public Handler(Callback callback, boolean async) {
	//......
    mLooper = Looper.myLooper();
    if (mLooper == null) {
        throw new RuntimeException(
            "Can't create handler inside thread that has not called Looper.prepare()");
    }
    mQueue = mLooper.mQueue;
    mCallback = callback;
    mAsynchronous = async;
}
```

先调用了 `Looper.myLooper()` 赋给 `mLooper`，点进去看看 `myLooper()` 方法的实现：

```java
/**
 * Return the Looper object associated with the current thread.  Returns
 * null if the calling thread is not associated with a Looper.
 */
public static @Nullable Looper myLooper() {
    return sThreadLocal.get();
}
```

很简单，返回一个与当前线程相关联的 `Looper` 对象。如果调用线程没有与 `Looper` 关联则返回 `null`。那怎么关联呢？往下看。

获取到  `Looper`  之后调用 `mLooper.mQueue` 获取到当前线程关联的 `Looper` 中的消息队列 `mQueue`。

`Looper` 中的消息队列 `mQueue` 是在 `Looper` 的构造方法中创建的：

```java
private Looper(boolean quitAllowed) {
    mQueue = new MessageQueue(quitAllowed);
    mThread = Thread.currentThread();
}
```

而 `Looper` 的构造方法是在哪儿调用的呢？看 `prepare()` 方法：

```java
 /** Initialize the current thread as a looper.
  * This gives you a chance to create handlers that then reference
  * this looper, before actually starting the loop. Be sure to call
  * {@link #loop()} after calling this method, and end it by calling
  * {@link #quit()}.
  */
public static void prepare() {
    prepare(true);
}

private static void prepare(boolean quitAllowed) {
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    sThreadLocal.set(new Looper(quitAllowed));
}
```

当 `prepare()` 方法被调用的时候，就会初始化一个 `Looper` 对象并与当前线程关联起来，而 `Looper` 中维护着一个消息队列 `MessageQueue` 。

当我们 `new` 一个 `Handler` 之后，调用 `sendMessage(message)` 方法或者是 `sendXXX` 等等诸如此类的方法发送消息，

最后都会走到 `sendMessageAtTime` 这个方法中，点进去看看它的实现：

```java
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        RuntimeException e = new RuntimeException(
                this + " sendMessageAtTime() called with no mQueue");
        Log.w("Looper", e.getMessage(), e);
        return false;
    }
    return enqueueMessage(queue, msg, uptimeMillis);
}
```

如果 `queue` 不为 `null` ，调用 `enqueueMessage` 方法，再点进去看看实现：

```java
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
    msg.target = this;
    if (mAsynchronous) {
        msg.setAsynchronous(true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);
}
```

在这里把 `this` 赋给了 `msg.target`，这里的`this` 就是当前的 `handler` 对象，`Message` 中的 `target` 就是 `Handler`，在这里就把 `Handler` 和 `Message` 关联起来了，然后调用 `MessageQueue` 中的 `enqueueMessage` 方法，把当前的消息传到消息队列中去：

```java
boolean enqueueMessage(Message msg, long when) {
    if (msg.target == null) {
        throw new IllegalArgumentException("Message must have a target.");
    }
    if (msg.isInUse()) {
        throw new IllegalStateException(msg + " This message is already in use.");
    }

    synchronized (this) {
        if (mQuitting) {
            IllegalStateException e = new IllegalStateException(
                    msg.target + " sending message to a Handler on a dead thread");
            Log.w(TAG, e.getMessage(), e);
            msg.recycle();
            return false;
        }

        msg.markInUse();
        msg.when = when;
        Message p = mMessages;
        boolean needWake;
        if (p == null || when == 0 || when < p.when) {
            // New head, wake up the event queue if blocked.
            msg.next = p;
            mMessages = msg;
            needWake = mBlocked;
        } else {
            // Inserted within the middle of the queue.  Usually we don't have to wake
            // up the event queue unless there is a barrier at the head of the queue
            // and the message is the earliest asynchronous message in the queue.
            needWake = mBlocked && p.target == null && msg.isAsynchronous();
            Message prev;
            for (;;) {
                prev = p;
                p = p.next;
                if (p == null || when < p.when) {
                    break;
                }
                if (needWake && p.isAsynchronous()) {
                    needWake = false;
                }
            }
            msg.next = p; // invariant: p == prev.next
            prev.next = msg;
        }

        // We can assume mPtr != 0 because mQuitting is false.
        if (needWake) {
            nativeWake(mPtr);
        }
    }
    return true;
}
```

消息已经发出去了，那么在哪儿接收呢？

调用 `Looper.loop()` 来循环消息队列：

```java
/**
 * Run the message queue in this thread. Be sure to call
 * {@link #quit()} to end the loop.
 */
public static void loop() {
    final Looper me = myLooper();
    if (me == null) {
        throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
    }
    final MessageQueue queue = me.mQueue;

    // Make sure the identity of this thread is that of the local process,
    // and keep track of what that identity token actually is.
    Binder.clearCallingIdentity();
    final long ident = Binder.clearCallingIdentity();

    for (;;) {
        Message msg = queue.next(); // might block
        if (msg == null) {
            // No message indicates that the message queue is quitting.
            return;
        }

        // This must be in a local variable, in case a UI event sets the logger
        final Printer logging = me.mLogging;
        if (logging != null) {
            logging.println(">>>>> Dispatching to " + msg.target + " " +
                    msg.callback + ": " + msg.what);
        }

        final long traceTag = me.mTraceTag;
        if (traceTag != 0 && Trace.isTagEnabled(traceTag)) {
            Trace.traceBegin(traceTag, msg.target.getTraceName(msg));
        }
        try {
            msg.target.dispatchMessage(msg);
        } finally {
            if (traceTag != 0) {
                Trace.traceEnd(traceTag);
            }
        }

        if (logging != null) {
            logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
        }

        // Make sure that during the course of dispatching the
        // identity of the thread wasn't corrupted.
        final long newIdent = Binder.clearCallingIdentity();
        if (ident != newIdent) {
            Log.wtf(TAG, "Thread identity changed from 0x"
                    + Long.toHexString(ident) + " to 0x"
                    + Long.toHexString(newIdent) + " while dispatching to "
                    + msg.target.getClass().getName() + " "
                    + msg.callback + " what=" + msg.what);
        }

        msg.recycleUnchecked();
    }
}
```

看36行，调用了 `msg.target.dispatchMessage(msg)` ，`msg.target` 就是 `Handler` ，就是调用了 `Handler` 的 `dispatchMessage` 方法来分发消息的：

```java
/**
 * Handle system messages here.
 */
public void dispatchMessage(Message msg) {
    if (msg.callback != null) {
        handleCallback(msg);
    } else {
        if (mCallback != null) {
            if (mCallback.handleMessage(msg)) {
                return;
            }
        }
        handleMessage(msg);
    }
}
```

`msg.callback` 是什么？是一个 `Runnable` ，如果指定了 `Message` 的 `callback` ，那就走 `handleCallback(msg)`：

```java
private static void handleCallback(Message message) {
    message.callback.run();
}
```

否则，如果 `mCallback != null` ，则执行 `mCallback.handleMessage(msg)` ：

```java
/**
 * Callback interface you can use when instantiating a Handler to avoid
 * having to implement your own subclass of Handler.
 *
 * @param msg A {@link android.os.Message Message} object
 * @return True if no further handling is desired
 */
public interface Callback {
    public boolean handleMessage(Message msg);
}
```

如果 `mCallback.handleMessage(msg)` 返回 `true`，则执行完毕，否则，再执行最后的 `handleMessage(msg)` ：

```java
/**
 * Subclasses must implement this to receive messages.
 */
public void handleMessage(Message msg) {
}
```

这个 `handleMessage` 就是我们创建 `Handler` 的时候重写的那个 `handleMessage` ，重写这个方法来处理接受到消息后的逻辑：

```java
private Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
		//TODO
    }
};
//或者
private static class MyHandler extends Handler {
    @Override
    public void handleMessage(Message msg) {
		//TODO
    }
}
```

到此，整个流程就走完了，整理一下：

1. `Looper.prepare()` ：调用 `Looper.prepare()` 的时候创建一个包含 `MessageQueue` 的 `Looper` 对象与当前线程关联；
2. `new Handler()` ：在构造方法中通过 `Looper.myLooper()` 获取当前线程的 `Looper` 对象，然后获取了 Looper 中的 MessageQueue；
3. 调用 `sendXXX()` : 会调用 `enqueueMessage()` ,把当前 `Handler` 实例赋给 `Message` 的 `target` ，最终调用 `MessageQueue` 的 `enqueueMessage()` ,把这个 Message 放进消息队列中；
4. 调用 `Looper.loop()` 循环消息队列，`msg.target.dispatchMessage(msg)` 分发消息；
5. 消息处理。

有人有疑问我们并没有调用 `Looper.prepare()` 和 `Looper.loop()` ，那是因为在 `ActivityThread` 的 `main` 方法中已经为我们写好了：

```java
public static void main(String[] args) {
    Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");
    SamplingProfilerIntegration.start();

    // CloseGuard defaults to true and can be quite spammy.  We
    // disable it here, but selectively enable it later (via
    // StrictMode) on debug builds, but using DropBox, not logs.
    CloseGuard.setEnabled(false);

    Environment.initForCurrentUser();

    // Set the reporter for event logging in libcore
    EventLogger.setReporter(new EventLoggingReporter());

    // Make sure TrustedCertificateStore looks in the right place for CA certificates
    final File configDir = Environment.getUserConfigDirectory(UserHandle.myUserId());
    TrustedCertificateStore.setDefaultUserDirectory(configDir);

    Process.setArgV0("<pre-initialized>");

    Looper.prepareMainLooper();

    ActivityThread thread = new ActivityThread();
    thread.attach(false);

    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();
    }

    if (false) {
        Looper.myLooper().setMessageLogging(new
                LogPrinter(Log.DEBUG, "ActivityThread"));
    }

    // End of event ActivityThreadMain.
    Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
    Looper.loop();

    throw new RuntimeException("Main thread loop unexpectedly exited");
}
```

在21行调用了 `Looper.prepareMainLooper()` ：

```java
public static void prepareMainLooper() {
    prepare(false);
    synchronized (Looper.class) {
        if (sMainLooper != null) {
            throw new IllegalStateException("The main Looper has already been prepared.");
        }
        sMainLooper = myLooper();
    }
}
```

创建了唯一的 `sMainLooper` 实例，然后在37行调用了 `Looper.loop()` 。（PS：所以在主线程调用 `Looper.myLooper()` 和 `Looper.getMainLooper()`是一样的）



关于 `Handler` 的  `post()` ：

```java
new Handler().postDelayed(new Runnable() {
    @Override
    public void run() {
		//TODO
    }
}, 2000);
```
```java
public final boolean post(Runnable r) {
   return  sendMessageDelayed(getPostMessage(r), 0);
}
```

通过 `getPostMessage` 创建一个 `Message` ，并把 `runnable` 传给了 `Message` 的 `callback` ：

```java
private static Message getPostMessage(Runnable r) {
    Message m = Message.obtain();
    m.callback = r;
    return m;
}
```

然后调用 sendMessageXXX -> sendMessageAtTime -> enqueueMessage -> MessageQueue enqueueMessage …… 后面流程都一样了，然后在 `dispatchMessage` 的时候，因为 `msg.callback != null` ,所以就执行了 `handleCallback(msg)` ：

```java
private static void handleCallback(Message message) {
    message.callback.run();
}
```

 就是执行了 `Runnable` 的 `run()` 。



完毕。