##@AfterViews
注解`@AfterViews`应该在views的绑定完成之后进行执行。

>当`onCreate()`被调用的时候，被`@ViewById`注解的成员属性还未被初始化。因此，我们可以使用`@AfterViews`来注解依赖views的方法。

栗子：

```java
@EActivity(R.layout.main)
public class MyActivity extends Activity {

    @ViewById
    TextView myTextView;

    @AfterViews
    void updateTextWithDate() {
        myTextView.setText("Date: " + new Date());
    }
[...]
```

你能够为多个方法以`@AfterViews`进行注解。注意：不要忘记，千万不要在`onCreate（）`方法中使用任何view相关的成员属性。

```java
@EActivity(R.layout.main)
public class MyActivity extends Activity {

    @ViewById
    TextView myTextView;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // DON'T DO THIS !! It will throw a NullPointerException, myTextView is not set yet.
        // myTextView.setText("Date: " + new Date());
    }
[...]
```

注意到注解是尽快的进行生效的。所以，在不需要和view相关的方法中使用类似`@Extra`，`@InstanceState`或者`@AfterView`的注解是没有任何问题的。

##@ViewsById
>since AndroidANnotation 3.1

该注解和ViewById类似，但是该注解会为多个Views进行注解，并且，该注解能够在`java.util.List`或者`android.view.View`的子类的成员属性上使用。注解的值应该是R.id.*的值，在注解结束之后，List中的成员就能够使用了。注意，不要将空值null放置入List中。

栗子：

```java
@EActivity
public class MyActivity extends Activity {

  @ViewsById({R.id.myTextView1, R.id.myOtherTextView})
  List<TextView> textViews;

  @AfterViews
  void updateTextWithDate() {
    for (TextView textView : textViews) {
      textView.setText("Date: " + new Date());
    }
  }
}
```



