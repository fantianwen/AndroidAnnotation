##NoneConfigurationInstance
>since AndroidAnnotation 2.5

当配置发生改变的时候，你的activities通过destroyed或者recreated。这时候，需要reload一些资源。但是你通常需要迁移配置来。。。

当你使用 `Activity.onRetainNonConfigurationInstance() `时，你可以使用返回的object来重置状态。

###1、@NonConfigurationInstance
当你为你的activity中的fields以@Nonconfiguration来保存信息。

```java
@EActivity
public class MyActivity extends Activity {

  @NonConfigurationInstance
  Bitmap someBitmap;

  @NonConfigurationInstance
  @Bean
  MyBackgroundTask myBackgroundTask;

}
```

>Caution:当你使用这个注解的时候，不能够注解那些和`Context`相关的对象，例如Drawable，Adapter，View这些。因为可能造成内存泄露，一旦发生了内存泄露，那么GC无法回收对象，这将会损耗相当部分的内存。
>
>该警告（Caution）不适用于注解@Bean，因为AA会自动照看并重新绑定Context。

