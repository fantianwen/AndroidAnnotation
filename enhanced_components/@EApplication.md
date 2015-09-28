##加强Appliaction
>since AndroidAnnotation 2.4

你可以通过`@EApplication`来强化`Application`类。

```
@EApplication
public class MyApplication extends Application {

}
```

在Application类中，除了那些和view相关的，你都可以使用AA annotations

```
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

##将Application类注入其他类中
>since AndroidAnnotation 2.1

你可以使用@App在其他类中进行Application类的注入

```java
@EActivity
public class MyActivity extends Activity {

  @App
  MyApplication application;

}
```

在其他使用了注解的组件中同样可以起作用，譬如这个使用了`@EBean`的Bean中：

```java
@EBean
public class MyBean {

  @App
  MyApplication application;

}
```

>since AndroidAnnotation 3.0,Application类中必须强制以`@EApplication`进行注解。

