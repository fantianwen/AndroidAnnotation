##Extras
>since AndroidAnnotations 1.0

###1、@Extra
被注解以`@Extra`的成员属性将会在启动activity之时作为Intent中的Extra参数进行传递。

栗子：

```java
@EActivity
public class MyActivity extends Activity {

  @Extra("myStringExtra")
  String myMessage;

  @Extra("myDateExtra")
  Date myDateExtraWithDefaultValue = new Date();

}
```

>since AndroidAnnotation 2.6

如果你不声明Extra的参数，那么，成员属性的名称就是Extra的key.

```java
@EActivity
public class MyActivity extends Activity {

  // The name of the extra will be "myMessage"
  @Extra
  String myMessage;
}
```

注意：这里你同样可以使用builder链式方法进行构造：

```java
MyActivity_.intent().myMessage("hello").start() ;
```

###2、处理`onNewIntent()`
>since AndroidAnnotation 2.6

###3、在Extras注入结束之后执行代码
>since AndroidAnnotation 3.1

如果你需要在注入结束之后执行某些代码，你需要使用`@AfterExtras`这个注解。

```java
@EActivity
public class MyClass {

  @Extra
  String someExtra;

  @Extra
  int anotherExtra;


  @AfterExtras
  public void doSomethingAfterExtrasInjection() {
    // someExtra and anotherExtra are set to the value contained in the incoming intent
    // if an intent does not contain one of the extra values the field remains unchanged
  }

}
```

###4、警告
>如果在父类和子类中被`@AfterViews`，`@Afterinject`和`@AfterExtras`这些注解者拥有相同的名称，那么就是出现严重的bug。
>
>同样，需要注意的是，被`@AfterViews`，`-inject`和`-Extras`注解者有执行的先后顺序，但是被`@AfterXXX`注解的方法的执行顺序却没有先后顺序。
>
>[这里](https://github.com/excilys/androidannotations/wiki/%40AfterXXX-call-order)拥有更多的细节。


