##加强Application类
>since AndroidAnnotation 2.4

你可以使用@EApplication注解来为Android中的`Application`进行注解:

```java
@EApplication
public class MyApplication extends Application {

}
```

这样，你就可以使用大多数的AA注解了。处了那些和Views和Extras相关的注解:

```java
@EApplication
public class MyApplication extends Application {

  public void onCreate() {
    super.onCreate();
    initSomeStuff();
  }

  @SystemService
  NotificationManager notificationManager;

  @Bean
  MyEnhancedDatastore datastore;

  @RestService
  MyService myService;

  @OrmLiteDao(helper = DatabaseHelper.class, model = User.class)
  UserDao userDao;

  @Background
  void initSomeStuff() {
    // init some stuff in background
  }
}
```

##将Application类对象进行依赖注入
>since AndroidAnnotation 2.1

你能够使用@App进行依赖注入:

```java
@EActivity
public class MyActivity extends Activity {

  @App
  MyApplication application;

}
```

这同样适用于任何其他的注解组件，譬如`@EBean`:

```java
@EBean
public class MyBean {

  @App
  MyApplication application;

}
```
>从AndroidAnnotation 3.0开始，强制使用`@EApplication`来为application进行注解。



