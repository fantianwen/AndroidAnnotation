##Fragment相关
###1、对FragmentActivity的支持
当你使用FragmentActivity来代替Activiy的时候，使用`EActivity`不会有任何问题。
###2、支持Fragment
AndroidAnnotation支持`android.app.Fragment`和`android.support.v4.app.Fragment`,并会根据使用的不同自动进行切换。
###3、加强Fragments
使用`EFragment`

```java
@EFragment
public class MyFragment extends Fragment {

}
```

AndroidAnnotation会自动生成类`MyFragment_`。这样，在你的xml文件中，你需要使用**生成的类的名称**。

```java
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="horizontal" >

    <fragment
        android:id="@+id/myFragment"
        android:name="com.company.MyFragment_"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />

</LinearLayout>
```

代码书写：

```java
MyFragment fragment = new MyFragment_();
```

或者使用流式buider：

```java
MyFragment fragment = MyFragment_.builder().build();
```

现在，你能在Fragment中使用各种各样的annotation：

```java
@EFragment
public class MyFragment extends Fragment {
    @Bean
    SomeBean someBean;

    @ViewById
    TextView myTextView;

    @App
    MyApplication customApplication;

    @SystemService
    ActivityManager activityManager;

    @OrmLiteDao(helper = DatabaseHelper.class, model = User.class)
    UserDao userDao;

    @Click
    void myButton() {
    }

    @UiThread
    void uiThread() {

    }

    @AfterInject
    void calledAfterInjection() {
    }

    @AfterViews
    void calledAfterViewInjection() {
    }

    @Receiver(actions = "org.androidannotations.ACTION_1")
    protected void onAction1() {

    }
}
```
>View的注入和事件监听只能基于在Fragment内部的View。但请注意到，这并非`@EBean`被注入到fragments内部的原因，真正的原因是：They hava access to the activity views。

###4、Fragment的布局文件
标准的写法是覆写`onCreateView()`方法：

```java
@EFragment
public class MyFragment extends Fragment {
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.my_fragment_layout, container, false);
        return view;
    }
}
```

或者和`EActivity`一样，给`@EFragment`传递一个参数：

```java
@EFragment(R.layout.my_fragment_layout)
public class MyFragment extends Fragment {
}
```

如果你需要通过覆写onCreateView()方法来获得savedinstanceState
，你仍旧可以通过让该方法返回`null`来让androidannotation去安置该Fragment的layout文件。

```java
@EFragment(R.layout.my_fragment_layout)
public class MyFragment extends Fragment {
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return null;
    }
}
```

###5、强制进行layout注入
>从AndroidAnnotation 3.3开始

在某些情况下，比方说你的Fragment继承自`ListFragment`，这样，子类将会返回一个在`onCreateView()`中返回一个非空的view。这样你的注解可能就不起作用了，这样，forceLayoutInjection就应运而生了，如果被设置为**true**，则无论何种情况，注解均起作用，而默认的值为**false**。

```java
@EFragment(R.layout.my_custom_layout)
public class MyFragment extends ListFragment {
  // R.layout.my_custom_layout will be injected
}
```

###6、Fragments注入
或许你用了`@EActivity`,`@EFragment`,`@EView`,`@EViewGroup`,`@EBean`,使用`@FragmentById`或者`@FragmentByTag`。如果你不做任何特殊的参数传递，那么默认成员名称就是参数。
>我们强烈的推荐使用`@FragmentById`而不是`@FragmentByTag`.

请注意`@FragmentById`和`@FragmentByTag`只有在Fragment中才能进行注入。我们不能对Fragment进行创建，这些需要进行注入的Fragment必须早就已经在Activity中，或者在`onCreate()`方法中进行了创建。
>允许你在没有进行`@EFragment`注入的fragments进行相关行为的注入。

```java
@EActivity(R.layout.fragments)
public class MyFragmentActivity extends FragmentActivity {
  @FragmentById
  MyFragment myFragment;

  @FragmentById(R.id.myFragment)
  MyFragment myFragment2;

  @FragmentByTag
  MyFragment myFragmentTag;

  @FragmentByTag("myFragmentTag")
  MyFragment myFragmentTag2;
}
```

###7、`DialogFragment`s中的一些注意点
很不幸的是，如果一个Fragment使用了`@EFragment`注入，那么你将不能在`onCreateDialog()`中创建一个新的`Dialog`对象。你只需调用`super.onCreateDialog()`方法，该方法中返回的`Dialog`的对象将为你所用。在这之后，你可以使用通过`@AfterViews`注解的方法来继续调用初始化你的Views。

你可以参考[这里](https://github.com/excilys/androidannotations/issues/486#issuecomment-110672535)以了解更多！


