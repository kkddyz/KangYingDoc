话讲在前面ascii和iso-8859-1是一个东西。

参考文章

> [Web开发须知：URL编码与解码](https://www.cnblogs.com/liuhongfeng/p/5006341.html)
>
> [request.getParameter("参数名") 中文乱码解决方法](https://www.huaweicloud.com/articles/02a827f18fa1b19b09323a4a212ed75e.html)
>
> [UTF-8编码规则_sandyen的专栏-CSDN博客_utf8编码规则](https://blog.csdn.net/sandyen/article/details/1108168)
>
> 第一篇文章特别棒，值得细读。



## 问题描述

对于请求中的中文参数，服务器 getParameter()失败；控制台显示????



下面这个例子是我搞明白后写的，现在可以跳过。

```xml
解释一下，比如客户端是支持中文编码GBK的，但是浏览器端的系统不支持GBK
那么会产生什么问题呢？ 

试想一下，假设你现在用的电脑就是浏览器，你想给美国的一位友人发消息问候
那么当他打开邮件后会发生什么？？？

我们从编码的角度开始分析 : 任何编码的字符串本质都是一个byte[].

通过网络传输的信息本质也就是一个byte[] ,那么你可以在你的电脑上浏览中文信息,原因是什么？？

原因是，你的系统有对应的语言包。语言包通过约定的格式，将byte[]截取为一个个单位的字符编码。
通过查表，将编码值对应的字符显示在屏幕上。这就是你可以在屏幕上看到字符的原因。

回到刚才的问题，这个外国友人对你真挚的问候被传送到网络上之后，
原来的 "你好"变成了 [127,233,244,222,121,232](我瞎编的数值,utf-8的中文字符可能是3字节)

[127,233,244,222,121,232] 通过网络到达友人的电脑后，当你打开邮箱的那一刻很遗憾他看到的大概率就是 ????y?
原因是你的友人的系统使用的是iso-8859-1的编码(自己去百度iso-8859字符集)。
如果你的友人是其他国家的，那么他看到的东西会有些变化，但是无论如何他也不知道你是在问候他。
```

## 预备知识

### URL转码的原因

通常如果一样东西需要编码，说明这样东西并不适合传输。

- 原因多种多样，如Size过大，包含隐私数据...

- 对于Url来说，之所以要进行编码，是因为存在不安全字符

  1. 保留字符 ： Url中有些字符会引起歧义。

     - 例如，Url参数字符串中使用key=value键值对这样的形式来传参，键值对之间以&符号分隔，如/s?q=abc& ie=utf-8。如果你的value字符串中包含了=或者&，那么势必会造成接收Url的服务器解析错误，因此必须将引起歧义的&和= 符号进行转义，也就是对其进行编码。

     - 保留字符，如：`/`  `=` 会被转码

       ![image-20211002164353016](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20211002164353.png)

  2. 非法字符 :例如中文。

     - Url的编码格式采用的是ASCII码，而不是Unicode，这也就是说你不能在Url中包含任何非ASCII字符。
       - 这属于一种规范 -- url编码就是让其他字符比如中文，转码成合法字符



Url编码的原则就是使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符。

### URL 编码方式

---

#### 编码思想

转码的核心思想：使用安全的字符表示不安全的字符。

对于不安全的保留字符以及ASCII码中没有的字符：转换为使用%百分号加上两个安全字符的格式 — %`[0123456789ABCDEF]{2}` — 通过16进制数表示该字符的8bit字节

这里的`0123456789ABCDEF` 就是安全字符(就是16进制数)。%表示一个加密的URL字节的开始。

简单来说就是根据当前文件的编码集，将不安全字符序列 (替换)—> 安全字符序列，然后在每两个安全字符中间插入%分隔(因为两个安全字符才能完整的表示一个字节)。

---

#### 验证过程

例如，对url根据utf-8编码 ”中文“ ,使用 （[在线编码工具](https://tool.chinaz.com/tools/urlencode.aspx)）得到的结果为 `%E4%B8%AD%E6%96%87`  。显然UTF-8中一个中文字符占三个字节。

按照上面的方法，有了URL编码后的安全字符，可以反推出"中文"的UTF-8字节序列。

如果反推出的字节序列与实际的一致就说明这个方法没问题。

首先，反推URL的安全序列 (取补码是因为java中的byte类型范围是 -128 ~127，取完补码才是对应的字节数组）

中  —> E4 B8 AD （对应的16进制序列） —> 228 184  173 (取补码) —>  -28, -72, -83

文  —> E6 96 87  （对应的16进制序列） —> 230  150 135 (取补码) —>   -26, -106, -121

然后在使用utf-8的文件编码下，执行以下代码

```java
byte[] bytes = "中文".getBytes(StandardCharsets.UTF_8);
String str = Arrays.toString(bytes);
System.out.println(str);
```

输出结果为[-28, -72, -83, -26, -106, -121]

两者是一致的。

- 字符集在URL编码中扮演的角色

  由上面的内容，我们知道URL实现了将不安全的序列使用安全序列表示。

  但是实现的过程，无论是加密还是解码都需要指定编码集。

  百分号编码只是提供了一种防止不安全字符的思想，但是如何实现是依赖于字符集的。

  假设你使用的是chrome浏览器，非法字符会使用utf-8进行url编码。于是浏览器发出的 "中国" 就变成了 `%E4%B8%AD%E6%96%87`

  然后，toncat服务器会接收到这个字节序列，然后进行URL解码，解码的格式默认是ISO-8859-1；

  ISO-8859-1是单字节字符集，查表228 184  173  230  150 135 128完后的扩展位里面全是空的。

  ![image-20211002164619054](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20211002164619.png)

  这个时候打印出来的就是????表示这些字符编码表示的字符未定义。

- 不同浏览器URLencode使用的字符集不同，tomct解码使用的默认是iso-8859-1

  > [不同浏览器中URL的编码方式_u014785687的博客-CSDN博客_浏览器默认编码](https://blog.csdn.net/u014785687/article/details/74078512)

## 解决思路

由于不同的浏览器的URL加密方式不同，所以对应的解码方式也是不同的。

需要使用util工具类来实现的。

如果单单考虑chrome  使用的是utf8,为了避免getParameter()方法使用iso-8859-1读取参数，就得

1. 修改默认解码字符集：修改Tomcat/conf 目录下 server.xml

   ```xml
   <Connector URIEncoding="UTF-8" acceptCount="1500" connectionTimeout="20000" 
   enableLookups="false" maxSpareThreads="100" maxThreads="1000" minSpareThreads="25" 
   port="9082" protocol="HTTP/1.1" useBodyEncodingForURI="true"/>
   
   重点在 userBodyEncodingForURI 和 URIEncoding 这两个属性
   
   下面来解释一下这两个属性的意义
   
   useBodyEncodingForURI参数表示是否用request.setCharacterEncoding参数对URL提交的数据和表单中GET方式提交的数据进行重新编码，在默认情况下，该参数为false。
   
   URIEncoding参数指定对所有GET方式请求进行统一的重新编码（解码）的编码。
   
   URIEncoding和useBodyEncodingForURI区别是，
   
   URIEncoding是对所有GET方式的请求的数据进行统一的重新编码，
   
   而useBodyEncodingForURI则是根据响应该请求的页面的request.setCharacterEncoding参数对数据进行的重新编码，不同的页面可以有不同的重新编码的编码。
   ```

2. 对字符串转码 (工具类通过获取请求头的浏览器名称，使用对应的转码的编码)

   String str = new String(request.getParameter("参数名").getBytes("iso-8859-1"), "utf-8");

### 如果是MAVEN项目

>  注意一下是使用独立的tomcat还是使用的tomcat的jar包，如果是后者需要修改pom.xml

以前使用的本地的tomcat服务器软件，在配置中URIEncoding属性改了。

但是，这个项目用的maven，使用的是tomcat的jar包，所以没有配置URIEncoding

在pom.xml 配置tomcat插件的地方加上uriEncoding标签

```xml
<plugins> 
	<!--tomcat插件-->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.1</version>
                <configuration>
                    <port>80</port>
                    <path>/univ-Resource_bank</path>
                    <uriEncoding>UTF-8</uriEncoding>
                </configuration>
            </plugin>
</plugins>
```

