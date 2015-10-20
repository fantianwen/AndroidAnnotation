#WorkWithThreads
>since AndroidAnnotation 1.0

**和`AsyncTasks`说见鬼去吧！！！**

##@Backgroud

以该注解的方法表明：该方法将会在不同于UI线程（主线程）的一个线程中运行。

栗子：

```java
void myMethod() {
    someBackgroundWork("hello", 42);
}

@Background
void someBackgroundWork(String aParam, long anotherParam) {
    [...]
}
```

>该方法是运行在一个独立的线程中的，但是，这并不表明一个新的线程将会开启，以为我们使用了缓存的`thread pool executor`，该缓存的线程池将会抑制线程的增长。


##Id
>since AndroidAnnotation 3.0

如果你希望将将一个后台进程杀掉，你能够使用`id`这个域。每个进程否能够使用`BackgroundExecutor.cancelAll("id")`来杀掉。

```java
void myMethod() {
    someCancellableBackground("hello", 42);
    [...]
    boolean mayInterruptIfRunning = true;
    BackgroundExecutor.cancelAll("cancellable_task", mayInterruptIfRunning);
}

@Background(id="cancellable_task")
void someCancellableBackground(String aParam, long anotherParam) {
    [...]
}
```

##Serail
>since AndroidAnnotation 3.0

默认的情况是，`@Background`方法是**并行**的，如果你希望他们是**线性**执行的，你可以使用`serial`这个域。所有拥有**相同**的`serial`的方法将会线性执行。

```java
void myMethod() {
    for (int i = 0; i < 10; i++)
        someSequentialBackgroundMethod(i);
}

@Background(serial = "test")
void someSequentialBackgroundMethod(int i) {
    SystemClock.sleep(new Random().nextInt(2000)+1000);
    Log.d("AA", "value : " + i);
}
```

##Delay
>since AndroidAnnotation 3.0

如果你希望在一个background运行之前添加一个延迟，你可以使用`dalay`参数：

```java
@Background(delay=2000)
void doInBackgroundAfterTwoSeconds() {
}
```










