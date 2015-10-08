##Simple HTTPS
>since AndroidAnnotation 2.6

###1、@HttpClient
我们可以通过注解`@HttpsClient`,并传递配置的KeyStore，TrustStore和主机名称验证来简化一个HTTPS。

所有的参数都是可选的。

###2、用例

####SSL相互验证（两方验证）

下面是一个完整的形式，如果你希望取得客户端授权：

```java
@HttpsClient(trustStore=R.raw.cacerts, 
    trustStorePwd="changeit",
    keyStore=R.raw.client,
    keyStorePwd="secret",
    allowAllHostnames=false)
HttpClient httpsClient;
```

- trustStore:int,资源id，譬如你的饿trust store文件类似于`R.raw.cacerts.bks`,这是一个服务端信任的证书。
- trustStorePws：String,你的trustStore的密码。（默认是changeit）
- keyStore：int，客户端证书的私钥
- keyStorePwd:KeyStore的密码
- allowAllHostnames:boolean,为所有的TLS/SSL 主机民称进行授权（该值默认是true）。如果是false的话，那么，证书中的主机名称必须符合URL。


>Note: Prior to ICS, Android accept [Key|Trust]store only in BKS format (Bouncycastle Key Store)

####一个简单的SSL授权
>如果你的远程服务器使用自签名的证书，或者一个私有认证机构发布的证书。这将十分有用。

```java
@HttpsClient(trustStore=R.raw.mycacerts, 
    trustStorePwd="changeit")
HttpClient httpsClient;
```

####缺省
如果你不指证一个truststore文件，那么annotation将会使用位于`/system/etc/security/cacerts.bks`处的truststore。这将会连接和你签订过的服务器。

```java
@HttpsClient
HttpClient httpsClient;
```

###3、完整的栗子

```java
@EActivity
public class MyActivity extends Activity {

    @HttpsClient(trustStore=R.raw.cacerts,
        trustStorePwd="changeit", 
        hostnameVerif=true)
    HttpClient httpsClient;

    @AfterInject
    @Background
    public void securedRequest() {
        try {
            HttpGet httpget = new HttpGet("https://www.verisign.com/");
            HttpResponse response = httpsClient.execute(httpget);
            doSomethingWithResponse(response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @UiThread
    public void doSomethingWithResponse(HttpResponse resp) {
        Toast.makeText(this, "HTTP status " + resp.getStatusLine().getStatusCode(), Toast.LENGTH_LONG).show();
    }
}
```




