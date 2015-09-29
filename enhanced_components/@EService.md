##加强services
>since AndroidAnnotation 2.4

你可以通过`@EService`来为`Service`进行注解:

```java
@EService
public class MyService extends Service {

}
```

通过`@EService`进行注解后，你就可以使用大多数的AA注解了，除了那些和views及extras相关的一些注解。

```java
@EService
public class MyService extends IntentService {

  @SystemService
  NotificationManager notificationManager;

  @Bean
  MyEnhancedDatastore datastore;

  @RestService
  MyRestClient myRestClient;

  @OrmLiteDao(helper = DatabaseHelper.class, model = User.class)
  UserDao userDao;

  public MyService() {
      super(MyService.class.getSimpleName());
  }

  @Override
  protected void onHandleIntent(Intent intent) {
    // Do some stuff...

    showToast();
  }

  @UiThread
  void showToast() {
    Toast.makeText(getApplicationContext(), "Hello World!", Toast.LENGTH_LONG).show();
  }
}
```

你可以通过流式Builder启动一个service：

```java
MyService_.intent(getApplication()).start();
```

>since AndroidAnnotation 3.0

如果你希望停止这个service，我们可以通过下面的方法停止:

```java
MyService_.intent(getApplication()).stop();
```

