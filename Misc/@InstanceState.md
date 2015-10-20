#Save instance state
>since AndroidAnnotation 2.5

你可以通过给类中的成员属性以注解`@InstanceState`,这样在activity销毁时保存数据。

```java
@EActivity
public class MyActivity extends Activity {

    @InstanceState
    int someId;

    @InstanceState
    MySerializableBean bean;

}
```

注解的属性会在`onSaveInstanceState(Bundle)`方法中保存。所有保存的数据会在`onCreate(Bundle)`中被调用。

>since AndroidAnnotation 2.7

在`@EFragment`中使用`@InstanceState`注解。

