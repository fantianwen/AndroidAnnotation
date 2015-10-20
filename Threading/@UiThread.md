##@UiThread
使用该注解的方法，表明会运行在ui线程上。

栗子：

```java
void myMethod() {
    doInUiThread("hello", 42);
}

@UiThread
void doInUiThread(String aParam, long anotherParam) {
    [...]
}
```

这下，不会再有`AsyncTask<Param,Progress,Result>`!!

##Delay
如果你希望在方法运行之前有一个延迟，你可以使用`delay`参数：

```java
@UiThread(delay=2000)
void doInUiThreadAfterTwoSeconds() {
}
```

##Propagation(宣传)

在AA 3.0之前，`@Thread` 注解的方法会在handler queue中进行排队，这样，所有的方法将会在Ui线程中执行完毕。从AA 3.0开始，期于兼容性的考虑，我们同样是这样实现的。

但是，如果你希望能够优化Ui线程的调用，你可以将`propagation`参数设置成`REUSE`。经过这样的配置后，如果代码的当前线程是Ui线程了，那么代码会直接运行。如果不是，该线程将会在handler线程中运行。

参数的设置方法是：

```java
@UiThread(propagation = Propagation.REUSE)
void runInSameThreadIfOnUiThread() {
}
```

Note:如果启用了delay，propagation将会被忽略，该方法将会永远在handler execution queue中。

##Id
>since AndroidAnnotation 4.0

如果你希望取消一个UI线程的task，那么你可以使用`id`这个域。每个被id标注的UI线程将会通过`UiThreadExecutor.cancelAll("id")`取消：

```java
void myMethod() {
    someCancellableUiThreadMethod("hello", 42);
    [...]
    UiThreadExecutor.cancelAll("cancellable_task");
}

@UiThread(id="cancellable_task")
void someCancellableUiThreadMethod(String aParam, long anotherParam) {
    [...]
}
```

如果一个任务已经执行了，那么他将会被取消。通过`propagation REUSE`标注的任务不能拥有`id`，同样，不能取消。



