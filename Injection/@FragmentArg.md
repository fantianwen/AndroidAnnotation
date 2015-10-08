##FrgmentArg
>since AndroidAnnotation 2.7

###1、@FragmentArg
主要使用在参数传递上，和`Fragment Argument`是等价的。

栗子：

```java
@EFragment
public class MyFragment extends Fragment {

  @FragmentArg("myStringArgument")
  String myMessage;

  @FragmentArg
  String anotherStringArgument;

  @FragmentArg("myDateExtra")
  Date myDateArgumentWithDefaultValue = new Date();

}
```

那么我们在构造一个Fragment的时候可以这样进行构造:

```java
MyFragment myFragment = MyFragment_.builder()
  .myMessage("Hello")
  .anotherStringArgument("World")
  .build();
```

这样的方式和

```java
Fragment.setArguments(Bundle bundle)
```

是等价的。


