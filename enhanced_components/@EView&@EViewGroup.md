##加强普通的Views
当你需要进行自定义组件制作的时候，**@EView**和**@EViewGroup**是你需要使用的注解。

###1、为什么我们需要使用普通的组件？
当你注意到，你正在重复性的使用layout中的一部分，那么你或许正在做一些重复性的工作，这个时候，你该考虑封装一下，这会使得你的工作变得轻松。

###2、使用`@EView`的普通组件
>since AndroidAnnotation 2.4

你只需要新建一个继承了View的类，并以`@EView`注解之，那么你就可以使用注解了。

```java
@EView
public class CustomButton extends Button {

        @App
        MyApplication application;

        @StringRes
        String someStringResource;

    public CustomButton(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
}
```

你可以继续使用在layout文件中使用（***记住：千万不要忘记_***）:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

    <com.androidannotations.view.CustomButton_
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <!-- ... -->

</LinearLayout>
```

同样，你可以进行代码实现：

```java
CustomButton button = CustomButton_.build(context);
```

###3、使用@EViewGroup
>since AndroidAnnotation 2.2

####怎样创建？

首先，让我们创建一个layout文件:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android" >

    <ImageView
        android:id="@+id/image"
        android:layout_alignParentRight="true"
        android:layout_alignBottom="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/check" />

    <TextView
        android:id="@+id/title"
        android:layout_toLeftOf="@+id/image"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textColor="@android:color/white"
        android:textSize="12pt" />

    <TextView
        android:id="@+id/subtitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/title"
        android:textColor="#FFdedede"
        android:textSize="10pt" />

</merge>
```

看看怎么使用吧：

```java
@EViewGroup(R.layout.title_with_subtitle)
public class TitleWithSubtitle extends RelativeLayout {

    @ViewById
    protected TextView title, subtitle;

    public TitleWithSubtitle(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public void setTexts(String titleText, String subTitleText) {
        title.setText(titleText);
        subtitle.setText(subTitleText);
    }

}
```

是不是看起来挺简单的？

####如何使用？
能够在任何layout文件中使用：

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical" >

    <com.androidannotations.viewgroup.TitleWithSubtitle_
        android:id="@+id/firstTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <com.androidannotations.viewgroup.TitleWithSubtitle_
        android:id="@+id/secondTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <com.androidannotations.viewgroup.TitleWithSubtitle_
        android:id="@+id/thirdTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</LinearLayout>
```

>***记住，千万不要忘记写“_”!***

在代码中获取自定义的组件并使用：

```java
@EActivity(R.layout.main)
public class Main extends Activity {

    @ViewById
    protected TitleWithSubtitle firstTitle, secondTitle, thirdTitle;

    @AfterViews
    protected void init() {

        firstTitle.setTexts("decouple your code",
                "Hide the component logic from the code using it.");

        secondTitle.setTexts("write once, reuse anywhere",
                "Declare you component in multiple " +
                "places, just as easily as you " +
                "would put a single basic View.");

        thirdTitle.setTexts("Let's get stated!",
                "Let's see how AndroidAnnotations can make it easier!");
    }

}
```

更多的请自己实践吧！

