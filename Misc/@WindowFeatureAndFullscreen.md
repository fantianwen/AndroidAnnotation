#WindowFeatureAndFullScreen

##@WindowFeature
>since AndroidAnnotation 3.0

通过注解`@WindowFeature`，你能够定制你的Activity的window feature。

```java
@WindowFeature({ Window.FEATURE_NO_TITLE, Window.FEATURE_INDETERMINATE_PROGRESS })
@EActivity
public class MyActivity extends Activity {

}
```

##@Fullscreen

>since AndroidAnnotation 2.2

该注解添加了Window flag `LayoutParams.FLAG_FULLSCREEN`。

```java
@Fullscreen
public class MyActivity extends Activity {

}
```

