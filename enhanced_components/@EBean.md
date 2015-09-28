##加强Custom classes（一般类）
>since AndroiAnnotation2.4

###1、加强一般类

通过注解`@EBean`，你能在一个非标准android组建中使用注解。

栗子：

```java
@EBean
public class MyClass {

}
```

>需要注意的是，使用了`@EBean`注解的类的构造方法中要么没有构造参数（空构造方法），要么只能有一个`Context`类型的构造参数。

###2、在已经注解的类中使用该注解
你需要使用`@Bean`:

```java
@EBean
public class MyOtherClass {

  @Bean
  MyClass myClass;

}
```

注意到，你能在其他类中继续使用`MyOtherClass`:

```java
@EActivity
public class MyActivity extends Activity {

  @Bean
  MyOtherClass myOtherClass;

}
```

>除非被`@EBean`注解的类是singleton（单例），否则每次通过`@Bean`注解的都是一个新的实例对象。

需要注意的是，该注解生成的子类是final的，这表示，该类不能被继承。但是，你可以为你继承了原类的子类进行注解。这适用于所用的注解。

举个栗子：

```java
@EActivity
public class MyChildActivity extends MyActivity {

}
```

###3、为指定的成员属性注解
>since AndroidAnnotation 2.5

如果你希望在某个注解的类中使用某个接口的实现类那么你可以这样使用：

```java
@EActivity
public class MyActivity extends Activity {

    /* A MyImplementation instance will be injected. 
     * MyImplementation must be annotated with @EBean and implement MyInterface.
     */
    @Bean(MyImplementation.class)
    MyInterface myInterface;

}
```

栗子中,通过对`myInterface`注解,并传入一个`MyImpletation.class`,能够生成一个

```java
myInterface=new MyImpletation
```

这样的实现类的实例对象。

这样的好处是实现了解耦。注重于针对interface进行编程。

###4、支持的注解
你能在被`@EBean`注解的类中进行大多数AA annotation类的注入。

```java
@EBean
public class MyClass {

  @SystemService
  NotificationManager notificationManager;

  @UiThread
  void updateUI() {

  }

}
```

###5、和View相关的注解的使用
你能够使用能View相关的注解。

```java
@EBean
public class MyClass {

  @ViewById
  TextView myTextView;

  @Click(R.id.myButton)
  void handleButtonClick() {

  }

}
```

>需要注意的是：依赖于Myclass的android组件中的ContentView中必须要有`myTextView`和`myButton`,这样才能起作用。

###6、注入root context
当你希望将依赖于`@EBean`的android根组件注入时，使用`@RootContext`注解，请注意，你注入的`Context`的类型是否正确。

```java
@EBean
public class MyClass {

  @RootContext
  Context context;

  // Only injected if the root context is an activity
  @RootContext
  Activity activity;

  // Only injected if the root context is a service
  @RootContext
  Service service;

  // Only injected if the root context is an instance of MyActivity
  @RootContext
  MyActivity myActivity;

}
``` 

在下面的`MyActivity`中，上面的`Service`的实例值是null。

```java
@EActivity
public class MyActivity extends Activity {

  @Bean
  MyClass myClass;

}
```

###7、在注入结束之后执行代码
在类中使用了注解，并不意味着已经进行实例注入了（还没实例化）。如果你希望在依赖注入结束之后进行代码的执行，请使用`@AfterInject`注解。

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

###8、警告
>如果父类和子类中相同名称方法均使用了`@AfterViews`,`@AfterInject`,`@AfterExtras`注解，那么，将会出现bug，在[issue #591](https://github.com/excilys/androidannotations/issues/591)寻找更多的细节吧！

###9、使用域
>since AndroiAnnotation 2.5

AndroidAnnotations现阶段支持在两种域使用：

* 默认类型：每次一个bean注入的时候生成一个新的实例；
* 单例类型：只有第一次才会创建，下面每次使用都会重复使用该实例。

```java
@EBean(scope = Scope.Singleton)
public class MySingleton {

}
```

>注意点：
>
>* 如果你希望使用`Singleton`域，那么建议加在`contetx.getApplicationContext()`.之所以这样是因为该context在整个app中声明周期最长，不容易造成内从泄露。
>* 在使用`Singleton`域声明的`@EBean`中进行View的注入和View的事件绑定是不允许的，理由如上，容易出现内存泄露。

