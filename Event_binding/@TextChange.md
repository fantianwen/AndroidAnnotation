##TextChangeEvents
>since AndroidAnnotation 2.6

###1、@TextChange
该注解适用于由`android.text.TextWatcher.onTextChanged(CharSequence s, int start, int before, int count) `定义的方法上，当TextView或者其子类上的text发生改变的时候，该方法就会被触发。该注解的值应该是一个或者多个`R.id.*`（其指向应该是TextView或者其子类）；没有不进行设置的话，那么成员属性就应该是`R.id.*`的名字。该方法可能应有多个参数：

- 一个应该接收该事件的TextView; 
- 一个需要修改的值charSequence;
- 一个int类型的值，获取修改的text的初始位置；
- 一个int类型的值，获取修改之前的text的长度；
- 一个int类型的值，获取修改之后的text的长度；

一些`@TextChange`的栗子：

```java
@TextChange(R.id.helloTextView)
 void onTextChangesOnHelloTextView(CharSequence text, TextView hello, int before, int start, int count) {
    // Something Here
 }

 @TextChange
 void helloTextViewTextChanged(TextView hello) {
    // Something Here
 }

 @TextChange({R.id.editText, R.id.helloTextView})
 void onTextChangesOnSomeTextViews(TextView tv, CharSequence text) {
    // Something Here
 }

 @TextChange(R.id.helloTextView)
 void onTextChangesOnHelloTextView() {
    // Something Here
 }
```

###2、@BeforeTextChange
该注解和`android.text.TextWatcher.beforeTextChanged(CharSequence s, int start, int count, int after)`是等价的。作用于TextView或者TextView的子类。如果没有设定参数，那么方法的名称就是R.id.*的域名。该注解可以设定多个参数。

- 一个作用的TextView；
- 一个`CharSequence`，是text改变之前的值；
- int类型的变量，是变化的`CharSequence`的初始位置；
- int类型变量，是变化的字符串的数量；
- int类型变量，是字符串变化之后的长度。

栗子：

```java
@BeforeTextChange(R.id.helloTextView)
 void beforeTextChangedOnHelloTextView(TextView hello, CharSequence text, int start, int count, int after) {
    // Something Here
 }

 @BeforeTextChange
 void helloTextViewBeforeTextChanged(TextView hello) {
    // Something Here
 }

 @BeforeTextChange({R.id.editText, R.id.helloTextView})
 void beforeTextChangedOnSomeTextViews(TextView tv, CharSequence text) {
    // Something Here
 }

 @BeforeTextChange(R.id.helloTextView)
 void beforeTextChangedOnHelloTextView() {
    // Something Here
 }
```
###3、@AfterTextChange

###4、@EditorAction









