---
layout: post
title: "android studio问题总结"
date: "2016-07-29 16:10:50 +0800"
---

新建一个android studio工程，在ide环境下gradle同步成功，但是在终端模式下尝试运行gradlew命令，出现各种错误

问题一：

{% highlight java%}
$ ./gradlew  androidDependencies
To honour the JVM settings for this build a new JVM will be forked. Please consider using the daemon: https://docs.gradle.org/2.10/userguide/gradle_daemon.html.

FAILURE: Build failed with an exception.

* Where:
Build file '/Users/wx/WorkSpace/androiddemo/Demo/build.gradle' line: 1

* What went wrong:
A problem occurred evaluating project ':Demo'.
> java.lang.UnsupportedClassVersionError: com/android/build/gradle/AppPlugin : Unsupported major.minor version 52.0

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 6.696 secs
{% endhighlight %}

<!-- more -->
解决方案：

将工程的全局build.gradle修改：
{% highlight java%}
buildscript {
    repositories {
        flatDir {
            dirs 'lib'
        }
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.0-alpha6'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
{% endhighlight %}

改为

{% highlight java%}
buildscript {
    repositories {
        flatDir {
            dirs 'lib'
        }
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
{% endhighlight %}

貌似alpha版本不支持

继续运行命令出现问题二：

{% highlight java%}
  $ ./gradlew  androidDependencies
  To honour the JVM settings for this build a new JVM will be forked. Please consider using the daemon: https://docs.gradle.org/2.10/userguide/gradle_daemon.html.

  FAILURE: Build failed with an exception.

  * What went wrong:
  A problem occurred configuring project ':Demo'.
   Could not resolve all dependencies for configuration ':Demo:_debugCompile'.
   Could not find com.android.support.constraint:constraint-layout:1.0.0-alpha4.
     Searched in the following locations:
         https://jcenter.bintray.com/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.pom
         https://jcenter.bintray.com/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.jar
         file:/Users/wx/Library/Android/sdk/extras/android/m2repository/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.pom
         file:/Users/wx/Library/Android/sdk/extras/android/m2repository/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.jar
         file:/Users/wx/Library/Android/sdk/extras/google/m2repository/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.pom
         file:/Users/wx/Library/Android/sdk/extras/google/m2repository/com/android/support/constraint/constraint-layout/1.0.0-alpha4/constraint-layout-1.0.0-alpha4.jar
     Required by:
         androiddemo:Demo:unspecified

  * Try:
  Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

  BUILD FAILED

  Total time: 19.992 secs
{% endhighlight %}

主工程引用了constraint-layout，所以在终端下编译报错

解决方案：

{% highlight java%}
  compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha4'
{% endhighlight %}

改为
{% highlight java%}
  compile 'com.android.support.constraint:constraint-layout:1.0+'
{% endhighlight %}

终端运行成功。
