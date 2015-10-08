##注入HTML
>since AndroidAnnotation 2.2

如果你想在`TextView`中注入一个TextView，那么有两种待选方案:

 - `@FromHtml`
 - `@HtmlRes`
 
 假设你拥有如下的String资源:
 
 ```xml
 <?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello_html"><![CDATA[Hello <b>World</b>!]]></string>
</resources>
 ```
 
 ###1、@HtmlRes
 
 该注解和`@StringRes`类似，但是其结果是经过`HTML.fromHtml()`解释过的结果：
 
 ```java
 @EActivity
public class MyActivity extends Activity {

  // Injects R.string.hello_html
  @HtmlRes(R.string.hello_html)
  Spanned myHelloString;

  // Also injects R.string.hello_html
  @HtmlRes
  CharSequence helloHtml;

}
 ```
 
 >注意到：Spanned实现了CharSequence，你可以两者都使用。
 
 
 ###2、@FromHtml
 
 该注解必须被早已被ViewById注解的`TextView`使用：
 
 ```java
@EActivity
public class MyActivity extends Activity {

  @ViewById(R.id.my_text_view)
  @FromHtml(R.string.hello_html)
  TextView textView;

  // Injects R.string.hello_html into the R.id.hello_html view
  @ViewById
  @FromHtml
  TextView helloHtml;

}
 ```


