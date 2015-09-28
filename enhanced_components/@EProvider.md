##加强contentproviders
>since AndroidAnnotation 2.4

你可以使用`@EProvider`来对一个content provider(内容提供者)进行注解:

```java
@EProvider
public class MyContentProvider extends ContentProvider {

}
```

经过`@EProvider`注解后，你就可以使用大多数的AA annotation了，除了那些和views相关的，以及一些其他的注解:

```java
@EProvider
public class MyContentProvider extends ContentProvider {

  @SystemService
  NotificationManager notificationManager;

  @Bean
  MyEnhancedDatastore datastore;

  @OrmLiteDao(helper = DatabaseHelper.class, model = User.class)
  UserDao userDao;

  @UiThread
  void showToast() {
    Toast.makeText(getContext().getApplicationContext(), "Hello World!", Toast.LENGTH_LONG).show();
  }

  // ...
}
```





