##加强IntentService
>since AndroidAnnotation 3.0

你可以通过注解`@EIntentService`来简化其中的处理方式（通过使用`@ServiceAction`注解）,通过`@EService`注解的类中可以实现大多数的AA 注解。当然，出了那些和views及extras相关的注解。

```java
@EIntentService
public class MyIntentService extends IntentService {

    public MyIntentService() {
        super("MyIntentService");
    }

    @ServiceAction
    void mySimpleAction() {
        // ...
    }

    @ServiceAction
    void myAction(String param) {
        // ...
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        // Do nothing here
    }
}
```

你可以通过Builder链式方法来启动一个加强版的`IntentService`：

```java
MyIntentService_.intent(getApplication()) //
    .myAction("test") //
    .start();
```

如果你在链式启动方法中实现了多个action，那么*最后一个*action将会被执行。

>since AndroidAnnotation 3.3

**Note**:因为`IntentService#onHandleIntent `该方法是抽象的，你需要至少一个空实现。为了方便，你可以使用`AbstractIntentService`，我们已经为你实现好了。



