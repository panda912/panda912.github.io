---
title: Android多线程之AsyncTask
date: 2017-02-10 14:59:12
tags:
comments: false
---

AsyncTask 的源码注释不要写的太详细哦。

AsyncTask enables proper and easy use of the UI thread. This class allows to perform background operations and publish results on the UI thread without having to manipulate threads and/or handlers.

AsyncTask is designed to be a helper class around `Thread` and `Handler` and does not constitute a generic threading framework. AsyncTasks should ideally be used for short operations (a few seconds at the most.) If you need to keep threads running for long periods of time,it is highly recommended you use the various APIs provided by the java.util.concurrent package such as `Executor`, `ThreadPoolExecutor` and `FutureTask`.

An asynchronous task is defined by a computation that runs on a background thread and whose result is published on the UI thread. An asynchronous task is defined by 3 generic types, called Params, Progress and Result, and 4 steps, called onPreExecute, doInBackground, onProgressUpdate and onPostExecute.

### Developer Guides

For more information about using tasks and threads, read the [Processes and Threads](https://developer.android.google.cn/guide/components/processes-and-threads.html#Processes) developer guide.

### Usage

AsyncTask must be subclassed to be used. The subclass will override at least one method (`doInBackground(Params...)`), and most often will override a second one (`onPostExecute(Result)`.)

Here is an example of subclassing:

```java
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
    protected Long doInBackground(URL... urls) {
        int count = urls.length;
        long totalSize = 0;
        for (int i = 0; i < count; i++) {
            totalSize += Downloader.downloadFile(urls[i]);
            publishProgress((int) ((i / (float) count) * 100));
            // Escape early if cancel() is called
            if (isCancelled()) break;
        }
        return totalSize;
    }

    protected void onProgressUpdate(Integer... progress) {
        setProgressPercent(progress[0]);
    }

    protected void onPostExecute(Long result) {
        showDialog("Downloaded " + result + " bytes");
    }
}
```

Once created, a task is executed very simply:

```java
new DownloadFilesTask().execute(url1, url2, url3);
```

### AsyncTask's generic types

The three types used by an asynchronous task are the following:

1. Params, the type of the parameters sent to the task upon execution.
2. Progress, the type of the progress units published during the background computation.
3. Result, the type of the result of the background computation.

Not all types are always used by an asynchronous task. To mark a type as unused, simply use the type `Void`:
 private class MyTask extends AsyncTask<Void, Void, Void> { ... }

### The 4 steps

When an asynchronous task is executed, the task goes through 4 steps:

1. `onPreExecute()`, invoked on the UI thread before the task is executed. This step is normally used to setup the task, for instance by showing a progress bar in the user interface.
2. `doInBackground(Params...)`, invoked on the background thread immediately after `onPreExecute()` finishes executing. This step is used to perform background computation that can take a long time. The parameters of the asynchronous task are passed to this step. The result of the computation must be returned by this step and will be passed back to the last step. This step can also use `publishProgress(Progress...)` to publish one or more units of progress. These values are published on the UI thread, in the `onProgressUpdate(Progress...)` step.
3. `onProgressUpdate(Progress...)`, invoked on the UI thread after a call to `publishProgress(Progress...)`. The timing of the execution is undefined. This method is used to display any form of progress in the user interface while the background computation is still executing. For instance, it can be used to animate a progress bar or show logs in a text field.
4. `onPostExecute(Result)`, invoked on the UI thread after the background computation finishes. The result of the background computation is passed to this step as a parameter.

### Cancelling a task

A task can be cancelled at any time by invoking `cancel(boolean)`. Invoking this method will cause subsequent calls to `isCancelled()` to return true. After invoking this method, `onCancelled(Object)`, instead of `onPostExecute(Object)` will be invoked after `doInBackground(Object[])` returns. **To ensure that a task is cancelled as quickly as possible, you should always check the return value of `isCancelled()` periodically from `doInBackground(Object[])`, if possible (inside a loop for instance.)**

### Threading rules

There are a few threading rules that must be followed for this class to work properly:

* The AsyncTask class must be loaded on the UI thread. This is done automatically as of `JELLY_BEAN`.
* The task instance must be created on the UI thread.
* `execute(Params...)` must be invoked on the UI thread.
* Do not call `onPreExecute()`, `onPostExecute(Result)`, `doInBackground(Params...)`, `onProgressUpdate(Progress...)` manually.
* The task can be executed only once (an exception will be thrown if a second execution is attempted.)

### Memory observability

AsyncTask guarantees that all callback calls are synchronized in such a way that the following operations are safe without explicit synchronizations.

* Set member fields in the constructor or `onPreExecute()`, and refer to them in `doInBackground(Params...)`.
* Set member fields in `doInBackground(Params...)`, and refer to them in `onProgressUpdate(Progress...)` and `onPostExecute(Result)`.

### Order of execution

When first introduced, AsyncTasks were executed serially on a single background thread. Starting with `DONUT`, this was changed to a pool of threads allowing multiple tasks to operate in parallel. Starting with `HONEYCOMB`, tasks are executed on a single thread to avoid common application errors caused by parallel execution.

If you truly want parallel execution, you can invoke `executeOnExecutor(java.util.concurrent.Executor, Object[])` with `THREAD_POOL_EXECUTOR`.