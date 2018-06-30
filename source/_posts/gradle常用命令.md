---
title: gradle常用命令
date: 2016-09-20 11:33:18
tags: gradle
---

### 查看依赖树

#### 查看所有依赖树

`gradle -q app:dependencies`

```
panda:Gank panda$ gradle -q app:dependencies

------------------------------------------------------------
Project :app
------------------------------------------------------------

_debugAndroidTestAnnotationProcessor - ## Internal use, do not manually configure ##
No dependencies

_debugAndroidTestApk - ## Internal use, do not manually configure ##
No dependencies

_debugAndroidTestCompile - ## Internal use, do not manually configure ##
No dependencies

_debugAnnotationProcessor - ## Internal use, do not manually configure ##
No dependencies

_debugApk - ## Internal use, do not manually configure ##
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1 -> 25.0.0
|    +--- com.android.support:support-v4:25.0.0
|    |    +--- com.android.support:support-compat:25.0.0
|    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:support-fragment:25.0.0
|    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    +--- com.android.support:support-vector-drawable:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:animated-vector-drawable:25.0.0
|         \--- com.android.support:support-vector-drawable:25.0.0 (*)
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:support-core-ui:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
+--- io.realm:realm-android-library:2.0.0
|    +--- io.realm:realm-annotations:2.0.0
|    \--- com.getkeepsafe.relinker:relinker:1.2.2
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android:1.4
     \--- com.squareup.leakcanary:leakcanary-analyzer:1.4
          +--- com.squareup.haha:haha:2.0.3
          \--- com.squareup.leakcanary:leakcanary-watcher:1.4

_debugCompile - ## Internal use, do not manually configure ##
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1 -> 25.0.0
|    +--- com.android.support:support-v4:25.0.0
|    |    +--- com.android.support:support-compat:25.0.0
|    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:support-fragment:25.0.0
|    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    +--- com.android.support:support-vector-drawable:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:animated-vector-drawable:25.0.0
|         \--- com.android.support:support-vector-drawable:25.0.0 (*)
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:support-core-ui:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
+--- io.realm:realm-android-library:2.0.0
|    +--- io.realm:realm-annotations:2.0.0
|    \--- com.getkeepsafe.relinker:relinker:1.2.2
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android:1.4
     \--- com.squareup.leakcanary:leakcanary-analyzer:1.4
          +--- com.squareup.haha:haha:2.0.3
          \--- com.squareup.leakcanary:leakcanary-watcher:1.4

_debugUnitTestAnnotationProcessor - ## Internal use, do not manually configure ##
No dependencies

_debugUnitTestApk - ## Internal use, do not manually configure ##
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

_debugUnitTestCompile - ## Internal use, do not manually configure ##
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

_releaseAnnotationProcessor - ## Internal use, do not manually configure ##
No dependencies

_releaseApk - ## Internal use, do not manually configure ##
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1 -> 25.0.0
|    +--- com.android.support:support-v4:25.0.0
|    |    +--- com.android.support:support-compat:25.0.0
|    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:support-fragment:25.0.0
|    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    +--- com.android.support:support-vector-drawable:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:animated-vector-drawable:25.0.0
|         \--- com.android.support:support-vector-drawable:25.0.0 (*)
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:support-core-ui:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
+--- io.realm:realm-android-library:2.0.0
|    +--- io.realm:realm-annotations:2.0.0
|    \--- com.getkeepsafe.relinker:relinker:1.2.2
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android-no-op:1.4

_releaseCompile - ## Internal use, do not manually configure ##
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1 -> 25.0.0
|    +--- com.android.support:support-v4:25.0.0
|    |    +--- com.android.support:support-compat:25.0.0
|    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:support-fragment:25.0.0
|    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    +--- com.android.support:support-vector-drawable:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:animated-vector-drawable:25.0.0
|         \--- com.android.support:support-vector-drawable:25.0.0 (*)
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    \--- com.android.support:support-core-ui:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
+--- io.realm:realm-android-library:2.0.0
|    +--- io.realm:realm-annotations:2.0.0
|    \--- com.getkeepsafe.relinker:relinker:1.2.2
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android-no-op:1.4

_releaseUnitTestAnnotationProcessor - ## Internal use, do not manually configure ##
No dependencies

_releaseUnitTestApk - ## Internal use, do not manually configure ##
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

_releaseUnitTestCompile - ## Internal use, do not manually configure ##
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

androidJacocoAgent - The Jacoco agent to use to get coverage data.
\--- org.jacoco:org.jacoco.agent:0.7.5.201505241946

androidJacocoAnt - The Jacoco ant tasks to use to get execute Gradle tasks.
\--- org.jacoco:org.jacoco.ant:0.7.5.201505241946
     +--- org.jacoco:org.jacoco.core:0.7.5.201505241946
     |    \--- org.ow2.asm:asm-debug-all:5.0.1
     +--- org.jacoco:org.jacoco.report:0.7.5.201505241946
     |    +--- org.jacoco:org.jacoco.core:0.7.5.201505241946 (*)
     |    \--- org.ow2.asm:asm-debug-all:5.0.1
     \--- org.jacoco:org.jacoco.agent:0.7.5.201505241946

androidTestAnnotationProcessor - Classpath for the annotation processor for 'androidTest'.
No dependencies

androidTestApk - Classpath packaged with the compiled 'androidTest' classes.
No dependencies

androidTestApt
+--- io.realm:realm-annotations:2.0.0
\--- io.realm:realm-annotations-processor:2.0.0
     +--- com.squareup:javawriter:2.5.0
     \--- io.realm:realm-annotations:2.0.0

androidTestCompile - Classpath for compiling the androidTest sources.
No dependencies

androidTestProvided - Classpath for only compiling the androidTest sources.
No dependencies

androidTestWearApp - Link to a wear app to embed for object 'androidTest'.
No dependencies

annotationProcessor - Classpath for the annotation processor for 'main'.
No dependencies

apk - Classpath packaged with the compiled 'main' classes.
No dependencies

apt
+--- io.realm:realm-annotations:2.0.0
+--- io.realm:realm-annotations-processor:2.0.0
|    +--- com.squareup:javawriter:2.5.0
|    \--- io.realm:realm-annotations:2.0.0
+--- com.jakewharton:butterknife-compiler:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    +--- com.google.auto:auto-common:0.6
|    |    \--- com.google.guava:guava:18.0
|    +--- com.google.auto.service:auto-service:1.0-rc2
|    |    +--- com.google.auto:auto-common:0.3 -> 0.6 (*)
|    |    \--- com.google.guava:guava:18.0
|    \--- com.squareup:javapoet:1.7.0
+--- com.android.support:appcompat-v7:24.2.1
|    \--- com.android.support:animated-vector-drawable:24.2.1
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0
|    |    \--- com.android.support:support-annotations:25.0.0
|    \--- com.android.support:support-core-ui:25.0.0
|         \--- com.android.support:support-compat:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-media-compat:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-utils:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-ui:25.0.0 (*)
|    \--- com.android.support:support-fragment:25.0.0
|         +--- com.android.support:support-compat:25.0.0 (*)
|         +--- com.android.support:support-media-compat:25.0.0 (*)
|         +--- com.android.support:support-core-ui:25.0.0 (*)
|         \--- com.android.support:support-core-utils:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0 (*)
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
\--- io.realm:realm-android-library:2.0.0
     +--- io.realm:realm-annotations:2.0.0
     \--- com.getkeepsafe.relinker:relinker:1.2.2

archives - Configuration for archive artifacts.
No dependencies

compile - Classpath for compiling the main sources.
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1
|    \--- com.android.support:animated-vector-drawable:24.2.1
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0
|    |    \--- com.android.support:support-annotations:25.0.0
|    \--- com.android.support:support-core-ui:25.0.0
|         \--- com.android.support:support-compat:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-media-compat:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-utils:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-ui:25.0.0 (*)
|    \--- com.android.support:support-fragment:25.0.0
|         +--- com.android.support:support-compat:25.0.0 (*)
|         +--- com.android.support:support-media-compat:25.0.0 (*)
|         +--- com.android.support:support-core-ui:25.0.0 (*)
|         \--- com.android.support:support-core-utils:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
\--- io.realm:realm-android-library:2.0.0
     +--- io.realm:realm-annotations:2.0.0
     \--- com.getkeepsafe.relinker:relinker:1.2.2

debugAnnotationProcessor - Classpath for the annotation processor for 'debug'.
No dependencies

debugApk - Classpath packaged with the compiled 'debug' classes.
No dependencies

debugCompile - Classpath for compiling the debug sources.
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0
|    |    |    +--- com.android.support:support-compat:25.0.0
|    |    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    \--- com.android.support:support-fragment:25.0.0
|    |    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    |    +--- com.android.support:support-vector-drawable:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:animated-vector-drawable:25.0.0
|    |         \--- com.android.support:support-vector-drawable:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0
|    |    |    +--- com.android.support:support-annotations:25.0.0
|    |    |    +--- com.android.support:support-compat:25.0.0 (*)
|    |    |    \--- com.android.support:support-core-ui:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android:1.4
     \--- com.squareup.leakcanary:leakcanary-analyzer:1.4
          +--- com.squareup.haha:haha:2.0.3
          \--- com.squareup.leakcanary:leakcanary-watcher:1.4

debugProvided - Classpath for only compiling the debug sources.
No dependencies

debugWearApp - Link to a wear app to embed for object 'debug'.
No dependencies

default - Configuration for default artifacts.
No dependencies

default-mapping - Configuration for default mapping artifacts.
No dependencies

default-metadata - Metadata for the produced APKs.
No dependencies

provided - Classpath for only compiling the main sources.
No dependencies

releaseAnnotationProcessor - Classpath for the annotation processor for 'release'.
No dependencies

releaseApk - Classpath packaged with the compiled 'release' classes.
No dependencies

releaseCompile - Classpath for compiling the release sources.
+--- project :widget
|    +--- com.android.support:appcompat-v7:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0
|    |    |    +--- com.android.support:support-compat:25.0.0
|    |    |    |    \--- com.android.support:support-annotations:25.0.0
|    |    |    +--- com.android.support:support-media-compat:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    +--- com.android.support:support-core-utils:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    +--- com.android.support:support-core-ui:25.0.0
|    |    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    |    \--- com.android.support:support-fragment:25.0.0
|    |    |         +--- com.android.support:support-compat:25.0.0 (*)
|    |    |         +--- com.android.support:support-media-compat:25.0.0 (*)
|    |    |         +--- com.android.support:support-core-ui:25.0.0 (*)
|    |    |         \--- com.android.support:support-core-utils:25.0.0 (*)
|    |    +--- com.android.support:support-vector-drawable:25.0.0
|    |    |    \--- com.android.support:support-compat:25.0.0 (*)
|    |    \--- com.android.support:animated-vector-drawable:25.0.0
|    |         \--- com.android.support:support-vector-drawable:25.0.0 (*)
|    +--- com.android.support:design:25.0.0
|    |    +--- com.android.support:support-v4:25.0.0 (*)
|    |    +--- com.android.support:appcompat-v7:25.0.0 (*)
|    |    +--- com.android.support:recyclerview-v7:25.0.0
|    |    |    +--- com.android.support:support-annotations:25.0.0
|    |    |    +--- com.android.support:support-compat:25.0.0 (*)
|    |    |    \--- com.android.support:support-core-ui:25.0.0 (*)
|    |    \--- com.android.support:transition:25.0.0
|    |         \--- com.android.support:support-v4:25.0.0 (*)
|    \--- com.google.android:flexbox:0.2.3
|         \--- com.android.support:appcompat-v7:23.3.0 -> 25.0.0 (*)
\--- com.squareup.leakcanary:leakcanary-android-no-op:1.4

releaseProvided - Classpath for only compiling the release sources.
No dependencies

releaseWearApp - Link to a wear app to embed for object 'release'.
No dependencies

retrolambdaConfig
\--- net.orfjackal.retrolambda:retrolambda:2.1.0

testAnnotationProcessor - Classpath for the annotation processor for 'test'.
No dependencies

testApk - Classpath packaged with the compiled 'test' classes.
No dependencies

testApt
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

testCompile - Classpath for compiling the test sources.
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3

testDebugAnnotationProcessor - Classpath for the annotation processor for 'testDebug'.
No dependencies

testDebugApk - Classpath packaged with the compiled 'testDebug' classes.
No dependencies

testDebugCompile - Classpath for compiling the testDebug sources.
No dependencies

testDebugProvided - Classpath for only compiling the testDebug sources.
No dependencies

testDebugWearApp - Link to a wear app to embed for object 'testDebug'.
No dependencies

testProvided - Classpath for only compiling the test sources.
No dependencies

testReleaseAnnotationProcessor - Classpath for the annotation processor for 'testRelease'.
No dependencies

testReleaseApk - Classpath packaged with the compiled 'testRelease' classes.
No dependencies

testReleaseCompile - Classpath for compiling the testRelease sources.
No dependencies

testReleaseProvided - Classpath for only compiling the testRelease sources.
No dependencies

testReleaseWearApp - Link to a wear app to embed for object 'testRelease'.
No dependencies

testWearApp - Link to a wear app to embed for object 'test'.
No dependencies

wearApp - Link to a wear app to embed for object 'main'.
No dependencies
```

#### 查看指定依赖树

`gradle -q app:dependencies --configuration debug`

```
panda:Gank panda$ gradle -q app:dependencies --configuration compile

------------------------------------------------------------
Project :app
------------------------------------------------------------

compile - Classpath for compiling the main sources.
+--- io.realm:realm-annotations:2.0.0
+--- com.android.support:appcompat-v7:24.2.1
|    \--- com.android.support:animated-vector-drawable:24.2.1
+--- com.android.support:recyclerview-v7:25.0.0
|    +--- com.android.support:support-annotations:25.0.0
|    +--- com.android.support:support-compat:25.0.0
|    |    \--- com.android.support:support-annotations:25.0.0
|    \--- com.android.support:support-core-ui:25.0.0
|         \--- com.android.support:support-compat:25.0.0 (*)
+--- com.github.CymChad:BaseRecyclerViewAdapterHelper:2.2.9
+--- com.youzan:titan:0.2.5
|    \--- com.android.support:recyclerview-v7:23.3.0 -> 25.0.0 (*)
+--- com.android.support:customtabs:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.squareup.okhttp3:logging-interceptor:3.4.1
+--- com.squareup.retrofit2:retrofit:2.1.0
+--- com.squareup.retrofit2:converter-gson:2.1.0
|    \--- com.google.code.gson:gson:2.7
+--- com.squareup.retrofit2:adapter-rxjava:2.1.0
|    \--- io.reactivex:rxjava:1.1.5 -> 1.1.6
+--- jp.wasabeef:picasso-transformations:2.1.0
+--- io.reactivex:rxandroid:1.2.1
+--- xiaofei.library:hermes-eventbus:0.2.0
|    +--- xiaofei.library:concurrent-utils:0.1.4
|    +--- org.greenrobot:eventbus:3.0.0
|    \--- xiaofei.library:hermes:0.6.1
+--- com.facebook.stetho:stetho-okhttp3:1.4.1
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.android.support:support-v4:25.0.0
|    +--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-media-compat:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-utils:25.0.0
|    |    \--- com.android.support:support-compat:25.0.0 (*)
|    +--- com.android.support:support-core-ui:25.0.0 (*)
|    \--- com.android.support:support-fragment:25.0.0
|         +--- com.android.support:support-compat:25.0.0 (*)
|         +--- com.android.support:support-media-compat:25.0.0 (*)
|         +--- com.android.support:support-core-ui:25.0.0 (*)
|         \--- com.android.support:support-core-utils:25.0.0 (*)
+--- com.android.support:cardview-v7:25.0.0
|    \--- com.android.support:support-annotations:25.0.0
+--- com.jakewharton:butterknife:8.3.0
|    +--- com.jakewharton:butterknife-annotations:8.3.0
|    |    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
|    \--- com.android.support:support-annotations:24.1.0 -> 25.0.0
+--- org.jsoup:jsoup:1.9.2
+--- com.google.code.gson:gson:2.7
+--- com.squareup.okhttp3:okhttp:3.4.1
|    \--- com.squareup.okio:okio:1.9.0
+--- com.squareup.picasso:picasso:2.5.2
+--- io.reactivex:rxjava:1.1.6
+--- com.tencent.bugly:crashreport:2.2.2
+--- com.facebook.stetho:stetho:1.4.1
|    +--- commons-cli:commons-cli:1.2
|    \--- com.google.code.findbugs:jsr305:2.0.1
+--- com.facebook.stetho:stetho-js-rhino:1.4.1
|    +--- com.google.code.findbugs:jsr305:2.0.1
|    \--- org.mozilla:rhino:1.7.6
\--- io.realm:realm-android-library:2.0.0
     +--- io.realm:realm-annotations:2.0.0
     \--- com.getkeepsafe.relinker:relinker:1.2.2

(*) - dependencies omitted (listed previously)
```

### 构建

先了解几个概念:

1. `Build Type`: 构建类型。一般是 debug 和 release，也可以在 module 的 build.gradle 中自定义：

   ```groovy
   android {
   	......
   	......
       buildTypes {
           debug {
               buildConfigField "String", "API_HOST", '"http://gank.io/api/"'
               debuggable true
               minifyEnabled false
               shrinkResources false
               zipAlignEnabled false
               signingConfig signingConfigs.debug
           }
           release {
               buildConfigField "String", "API_HOST", '"http://gank.io/api/"'
               minifyEnabled true
               shrinkResources true
               zipAlignEnabled true
               signingConfig signingConfigs.release
               proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
           }
       }
     	......
   	......
   }
   ```

2. `Product Flavor`: 渠道名  比如下面我们配置的`yingyongbao`、`wandoujia`、`beta`

   ```groovy
   productFlavors {
       yingyongbao {
           applicationId "com.sgb.gank"
           manifestPlaceholders = [APP_NAME: "Gank", CHANNEL_VALUE: 'yingyongbao', CHANNEL_ID: 1]
       }
       wandoujia {
           applicationId "com.sgb.gank"
           manifestPlaceholders = [APP_NAME: "Gank", CHANNEL_VALUE: 'wandoujia', CHANNEL_ID: 2]
       }
       beta {
           applicationId "com.sgb.gank.beta"
           manifestPlaceholders = [APP_NAME: "Gank.beta"]
       }
   }
   ```

3. `Build Variant`: `Product Flavor`和`Build Type`的排列组合`

   ![](1.png)

#### assemble+BuildType

`./gradlew assembleRelease` 构建所有渠道的 release 版本

![](2.png)

#### assemble+ProductFlavor

`./gradlew assembleYingyongbao` 构建指定渠道的所有 Build Type 版本

![](3.png)

#### asselmbe+BuildVariant

`./gradlew assembleYingyongbaoRelease` 构建指定渠道的指定版本

![](4.png)

未完待续。。。

