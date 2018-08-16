## Xposed入门 

### 工作原理

开始动手之前，应该大体了解一下Xposed工作原理，觉得无聊可以跳过。

做Android应用开发的可能都知道，有一个根本进程“Zygote”，是Android运行时的核心。每一个进程都是从这个核心进程中复刻出来的，进程启动使用`/system/bin/app_process` 加载所需的类并调用初始化方法。

这就是Xposed起作用的地方。安装好了Xposed框架以后，会有一个扩展的app_process会被复制到`/system/bin`中。这个扩展的启动进程会添加额外的jar到classpath并且调用一些在某些位置上的方法。比如，VM刚刚被创建完，甚至在zygote的main方法被调用之前。

### 方法可以被挂钩(Hook)/替換

为方法Hook让Xposed具有强大的力量。当你在反编译apk，你可以直接插入/修改任何你想要修改的地方。但是，这需要在修改了以后重新编译并签名apk，还要重新分发apk包。使用Xposed钩子，就不需要修改方法内的代码，而选择在方法执行的前后注入自己的代码。

XposedBridge  有一个私有的、本地化的方法`hookMethodNative` 。这个方法也在扩展的`app_process`中实现。它会修改方法类型并且链接到自己实现的通用方法。这意味着每次被hook的方法被调用时，通用方法可以让调用者无感知的调用这个通用方法。在这个方法中，XposedBridge 当中 `handleHookedMethod`  会被调用，把方法调用的的参数传递过去，以及this引用。这个方法会调用已注册的方法。这样可以修改调用参数、更改实例、静态变量，调用其它方法等等，非常灵活。

### 创建项目

一个模块就是一个App，只是多了几个metadata和文件。

创建一个Android Studio项目，添加Xposed 框架API到项目中。

```groovy
 compileOnly 'de.robv.android.xposed:api:82'
```

在清单文件中，加入几个metadata:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="de.robv.android.xposed.mods.tutorial"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="15" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <meta-data
            android:name="xposedmodule"
            android:value="true" />
        <meta-data
            android:name="xposeddescription"
            android:value="这是当前xposed模块的描述" />
        <meta-data
            android:name="xposedminversion"
            android:value="53" />
    </application>
</manifest>
```

### 实现一个hook模块

增加一个类在：

```java
package de.robv.android.xposed.mods.tutorial;

public class Tutorial {

}
```

对于要运行时修改行为来说，我们需要实现一个接口，让hook的方法在执行时执行我们的方法，而我们需要提供一个入口。并且提供被调用的方法。

```java
package de.robv.android.xposed.mods.tutorial;

import de.robv.android.xposed.IXposedHookLoadPackage;
import de.robv.android.xposed.XposedBridge;
import de.robv.android.xposed.callbacks.XC_LoadPackage.LoadPackageParam;

public class Tutorial implements IXposedHookLoadPackage {
    public void handleLoadPackage(final LoadPackageParam lpparam) throws Throwable {
        XposedBridge.log("Loaded app: " + lpparam.packageName);
    }
}
```

如此一来，我们就写了一个监听加载包的接口。 XposedBridge.log可以将日志记录在：

`/data/data/de.robv.android.xposed.installer/log/debug.log `中。

### xposed_init文件

有了要执行的入口代码，我们还需要告诉xposed框架，入口在哪里。

在`assets`文件夹中创建一个`xposed_init`文件，在这个文件中，`每一行`都包含一个完整的类名，作为入口类。



### 定制你的系统

通过这种方式可以轻松的定制您手机的系统(！！！小心操作！！！)，可以不通过下载源码编译ROM的方式，通过Xposed轻松达到定制系统UI、行为的目的。



### 使用反射来找到并且挂钩方法

如果我们需要对特定的包当中的某个方法进行hook操作，那么我们可以针对包进行判断:

以系统的ui包为例

```java
public void handleLoadPackage(LoadPackageParam lpparam) throws Throwable {
    if (!lpparam.packageName.equals("com.android.systemui"))
        return;

    XposedBridge.log("we are in SystemUI!");
}
```

`LoadPackageParam `参数可以提供一些信息，来确保我们要操作的包是不是正确的包。如果验证通过，那么我们就可以获得这个包的`ClassLoader`在这个参数中的变量引用。

我们现在打算修改一下系统时间的行为，我们寻找到`com.android.systemui.statusbar.policy.Clock` 类和`updateClock`  方法，然后要让XposedBridge去挂钩：

```java
package de.robv.android.xposed.mods.tutorial;

import static de.robv.android.xposed.XposedHelpers.findAndHookMethod;
import de.robv.android.xposed.IXposedHookLoadPackage;
import de.robv.android.xposed.XC_MethodHook;
import de.robv.android.xposed.callbacks.XC_LoadPackage.LoadPackageParam;

public class Tutorial implements IXposedHookLoadPackage {
    public void handleLoadPackage(final LoadPackageParam lpparam) throws Throwable {
    	if (!lpparam.packageName.equals("com.android.systemui"))
            return;
    	
    	findAndHookMethod("com.android.systemui.statusbar.policy.Clock", lpparam.classLoader, "updateClock", new XC_MethodHook() {
    		@Override
    		protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
    			// this will be called before the clock was updated by the original method
    		}
    		@Override
    		protected void afterHookedMethod(MethodHookParam param) throws Throwable {
    			// this will be called after the clock was updated by the original method
    		}
	});
    }
}
```

`findAndHookMethod`  是一个辅助函数。这个函数是静态导入的方式。这个方法在SystemUI包当中的ClassLoader中寻找`Clock`  类。然后寻找`updateClock`  方法。如果有参数，需要列举参数类型。`findAndHookMethod`  最后一个参数，需要提供一个`XC_MethodHook`  实现类。

在实现这个类时，有两个方法需要实现，`beforeHookedMethod` 和`afterHookedMethod`  ，不难猜测这两个函数的意思，就是执行原函数前后执行的函数。可以在before函数中操作原函数的参数甚至不执行原函数来返回自己定义的返回值。“after”会根据原始方法执行一些操作。可以在这里操纵返回结果。

> 如果你想完全替换一个方法，可以看一下子类`XC_MethodReplacement`  ，这个子类只需要重载`replaceHookedMethod` 

XposedBridge为每个挂钩方法保留一个已注册的回调列表。 优先级最高的那些（在hookMethod中定义）首先被调用。原始方法始终具有最低优先级。 因此，如果你使用回调A（prio high）和B（prio  default）挂钩一个方法，那么每当调用hooked方法时，控制流将是：A.before  - > B.before  - >  original method  - > B.after  - > A.after。 



##  结合VirtualXposed来开发Xposed Module

VirtualXposed可以无需ROOT来将应用运行在虚拟APP空间中，有自己的vm空间。



[参考来源]:1

1.https://github.com/rovo89/XposedBridge/wiki/Development-tutorial

2.https://github.com/android-hacker/VirtualXposed/wiki/Utilities-For-Xposed-Module-Developer

