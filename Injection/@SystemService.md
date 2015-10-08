##SystemServices
>since AndroidAnnotation 1.0

###1、标准Android System Service 注入
获取一个Android System Service需要记住服务的名字，并且进行类型强转。

```java
notificationManager = (NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE);
```

###2、SystemService
使用了该注解就代表，该成员属性就是一个android system service。

这和使用Context.getSystemService()是等价的。

栗子：

```java
@EActivity
public class MyActivity extends Activity {

  @SystemService
  NotificationManager notificationManager;

}
```




