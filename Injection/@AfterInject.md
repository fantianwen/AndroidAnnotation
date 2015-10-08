##在依赖注入之后执行代码
当被以`@EBean`注解的类的构造方法被执行的时候，其成员属性还并没有被注入。如果你希望在依赖注入之完，编译阶段就执行相关的代码，那么你需要在需要执行的方法上使用`@AfterInject`注解。

```java
@EBean
public class MyClass {

  @SystemService
  NotificationManager notificationManager;

  @Bean
  MyOtherClass dependency;

  public MyClass() {
    // notificationManager and dependency are null
  }

  @AfterInject
  public void doSomethingAfterInjection() {
    // notificationManager and dependency are set
  }

}
```






