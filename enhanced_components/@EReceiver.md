##加强broadcastreceiver（广播接收者）
>since AndroidAnnotation 2.4

你可以通过注解`@EReceiver`来为你的BroadcastReceiver进行注解：

```java
@EReceiver
public class MyReceiver extends BroadcastReceiver {

}
```

经过注解的广播接收者可以使用大多数的AA annotations，除了那些和views相关的，以及一些其他的注解：

```java
@EReceiver
public class MyReceiver extends BroadcastReceiver {

  @SystemService
  NotificationManager notificationManager;

  @Bean
  SomeObject someObject;

}
```

###1、**@ReceiverAction**
>since AndroidAnnotation 3.2

`@ReceiverAction`该注解能够帮你拥有`@Receiver`注解强化的广播接收者中简化处理那些广播事务。

如同之前的方法名就是该注解需要传递的参数一样，`@ReceiverAction`也默认拥有这样的参数传递方式，同时，你也能够为该注解进行参数传递。

通过`@ReceiverAction`注解的方法拥有下面的几种参数：

- 一个`android.content.Context`可以成为方法`void onReceiver(Context context,Intent intent)`的参数;
- 一个`android.content.Intent`可以成为方法`void onReceiver(Context context,Intent intent)`的参数;
- 任意的native（本地的），`android.os.Parcelable`或者`java.io.Seriable`的实现类（Bean），只要通过`@ReceiverAction.Extra`注解，该`Bean`的对象将成为Intent中传递的额外的（Extra）参数。_*注意的是，该extra参数的key：如果没有设置注解的参数的话就是方法中的参数名称，有的话就是注解中传递的名称。*_

```java
@EReceiver
public class MyIntentService extends BroadcastReceiver {

    @ReceiverAction("BROADCAST_ACTION_NAME")
    void mySimpleAction(Intent intent) {
        // ...
    }

    @ReceiverAction
    void myAction(@ReceiverAction.Extra String valueString, Context context) {
        // ...
    }

    @ReceiverAction
    void anotherAction(@ReceiverAction.Extra("specialExtraName") String valueString, @ReceiverAction.Extra long valueLong) {
        // ...
    }

    @Override
    public void onReceive(Context context, Intent intent) {
        // empty, will be overridden in generated subclass
    }
}
```

>since AndroidAnnotation 3.3

**Note**：因为`BroadcastReceiver#onReceive `该方法是抽象的，你必须添加一个空实现。为了方便，我们提供了 `AbstractBroadcastReceiver`，该类默认实现了抽象方法。

**Note**：现在你能够通过使用`@ReceiverAction`传递多个需要处理的action了.

```java
@ReceiverAction({"MULTI_BROADCAST_ACTION1", "MULTI_BROADCAST_ACTION2"})
    void multiAction(Intent intent) {
        // ...
    }
```

###2、数据范式
通过使用`dataScheme`的参数，你能够设置一个或者多个需要Receiver处理的数据范式。

```java
@EReceiver
public class MyIntentService extends BroadcastReceiver {

  @ReceiverAction(actions = android.content.Intent.VIEW, dataSchemes = "http")
  protected void onHttp() {
    // Will be called when an App wants to open a http website but not for https.
  }

  @ReceiverAction(actions = android.content.Intent.VIEW, dataSchemes = {"http", "https"})
  protected void onHttps() {
    // Will be called when an App wants to open a http or https website.
  }

}
```

###3、**`@Receiver annotation`**注解
或许你时常会在你的`Activity`中进行`BroadcastReceiver`的注册，现在你可以使用注解`@Receiver`来代替声明一个`BroadcastReceiver`。

```java
@EActivity
public class MyActivity extends Activity {

  @Receiver(actions = "org.androidannotations.ACTION_1")
  protected void onAction1() {

  }

}
```

每当一个拥有`actions = "org.androidannotations.ACTION_1"`的广播被发送的时候，都会执行方法`onAction1()`.






