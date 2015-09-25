#<p align="center" >Android Annotation</p>
#一、在android studio中使用android annotation
##在project工程文件下的build.gradle文件中

```xml
buildscript {
    repositories {
        jcenter ()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.3.0'

        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.4+'
    }
}

allprojects {
    repositories {
        jcenter ()
    }
}

```

##在app文件下的build.gradle文件中

```xml
apply plugin: 'com.android.application'
apply plugin: 'android-apt'
def AAVersion='3.2+'


android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
        applicationId "com.study.radasm.annotationdemo"
        minSdkVersion 17
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.0'
    apt "org.androidannotations:androidannotations:$AAVersion"
    compile "org.androidannotations:androidannotations-api:$AAVersion"
}

apt {
    arguments {
        androidManifestFile variant.outputs[0].processResources.manifestFile
        // if you have multiple outputs (when using splits), you may want to have other index than 0

        // You can set optional annotation processing options here, like these commented options:
        // logLevel 'INFO'
        // logFile '/var/log/aa.log'

    }
}

```

出现问题请`科学上网`！

#二、加强组件
##Activities相关
###1、@EActivity
该注解的参数必须是一个layout文件的id，该文件将用作该actiivty的Content View

你同样可以传递一个空的参数，这代表该activity不需要设置Content View,或者你希望在**绑定**之前，自己设置一个Content View。

栗子：		
	
```java
@EActivity(R.layout.main)
public class MyActivity extends Activity {

}
```

没有layout id：

```java
@EActivity
public class MyListActivity extends ListActivity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }

}
```

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












