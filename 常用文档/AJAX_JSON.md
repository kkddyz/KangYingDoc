

浏览器发送异步请求 -> 服务器处理请求返回json格式数据 -> 浏览器解析json







# 浏览器发送异步请求

## AJAX概念

含义：异步的 JS 和 XML

- 什么是异步？

  ![异步Ajax](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210706134015.png)
  
  > 传统方式中，浏览器发出请求会一直等待服务器的响应，就像卡住一样。
  
- 什么是AJAX 

  > **AJAX**即“**Asynchronous JavaScript and XML**”（异步的[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)与[XML](https://zh.wikipedia.org/wiki/XML)技术），指的是一套综合了多项技术的[浏览器](https://zh.wikipedia.org/wiki/瀏覽器)端[网页](https://zh.wikipedia.org/wiki/網頁)开发技术。Ajax的概念由[杰西·詹姆士·贾瑞特](https://zh.wikipedia.org/wiki/傑西·詹姆士·賈瑞特)所提出[[1\]](https://zh.wikipedia.org/wiki/AJAX#cite_note-1)。
  >
  > 传统的Web应用允许用户端填写表单（form），当提交表单时就向[网页服务器](https://zh.wikipedia.org/wiki/網頁伺服器)发送一个请求。服务器接收并处理传来的表单，然后送回一个新的网页，但这个做法浪费了许多带宽，因为在前后两个页面中的大部分[HTML](https://zh.wikipedia.org/wiki/HTML)码往往是相同的。由于每次应用的沟通都需要向服务器发送请求，应用的回应时间依赖于服务器的回应时间。这导致了用户界面的回应比本机应用慢得多。
  >
  > 与此不同，AJAX应用可以仅向服务器发送并取回必须的数据，并在客户端采用JavaScript处理来自服务器的回应。因为在服务器和浏览器之间交换的数据大量减少，服务器回应更快了。同时，很多的处理工作可以在发出请求的[客户端](https://zh.wikipedia.org/wiki/客户端)机器上完成，因此Web服务器的负荷也减少了。

  > 这里交换数据的格式就是JSON

---

## Jquery实现Ajax

1. $.ajax();

   *Syntax* : `$.ajax({K,V})`

   ```HTML
     <script src="https://code.jquery.com/jquery-3.1.1.min.js">
       function fun1() {
           //  使用 $.ajax()发送Ajax请求
           $.ajax({
                   // 通过K:V传递函数的参数
                   url: "ajaxServlet",
                   type: "POST",
                   // data: "username=jack&&age=23",
                   data: {"username": jack, "age": 23}, // json格式数据(h)
                   success:function (data) {
                       alert(data)
                   },//请求成功后的回调函数
                   error:function () {
                       alert("出错了...")
                   },//请求失败后的回调函数
                   dataType:"text" // 设置响应的数据格式
   
               }
           );
       }
   
   </script>
   ```

2. `$.get(url,[data],callback,type);`

   1. url : 请求路径
   2. data：请求参数
   3. callback：回调函数
   4. type : 数据返回类型

   ```HTML
    // 简化：不需要指定K 按顺序写值
           function fun2(){
               // 使用get方法 后面三个参数是可选的
               $.get(
                   "ajaxServlet", // 路径
                   {"username":"rose"},// 请求参数
                   function (date) {
                       alert(date);
                   }, // 回调函数
                   "text");
           }
   ```

3. `$.post(url,[data],callback,type);`

   - $.post()只是请求的方式不一样，其他没有区别；

---

# 浏览器解析JSON对象

## JSON概念

> JSON作为数据传输载体的时候称为JSON字符串。
>
> JSON对象是浏览器根据JSON语法将JSON字符串解析为数据结构，用以获取数据。
>
> JSON对象与Java中的对象含义不同，因为JSON对象只保存数据而不具有行为。
>
> 同时，JSON对象没有类型的概念。(JS是弱类型语言)

- Java Script Object Notation ，JavaScript对象表示法。

```
var p ={name :"张三",age : 23}
```

现在发展为存储和交换信息的语法；类似XML；

JSON比XML更加轻量，解析更容易。

---

## 基本语法

- <font color="yellow">JSON 语法是 JavaScript 对象表示法语法的子集。</font>

- 数据以键值对方式保存
  - 数据类型
    - 数字(整数或浮点数)
    - 字符串(双引号)
    - 逻辑值(true || false )
    - 数组 [] 可嵌套
      - 对象 {} 可嵌套
- 数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

---

## 简单使用

```JS
<script>
    alert("开始");

    //  定义一个person对象 而不是一个person类
    var person = {name:"张三",age:18};

    // 获取数据
    var name = person.name;
    alert(name);// 张三

    // 嵌套的对象
    var persons = {
        student:[
            {name:"李四",age:22},
            {name:"王五",age:23}
        ],
        teacher:[
            {name:"aa",age:33},
            {name:"bb",age:44}
        ]
    };

    // 获取数组元素
    var name1 = persons.student[0].name;
    alert(name1);// 李四

    // 遍历数组student
    for (var i = 0; i < persons.student.length; i++){
        alert("打印数组元素");
        // 遍历数组对象属性
        for (var key in persons.student[i]){
            alert(key + ": " + person[key]);// 怎么根据key获取value??
        }
    }


    alert("结束");

  </script>
```

> js中对象是没有类型检查的



---

# 服务器端解析参数处理请求

- JSON ->  Java -> JSON

## 引言

JSON中引入了对象和数据，方便了client-server的数据交互(建立起了JSON和JAVA对象下hi见的映射)。

![image-20210706165632873](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210706165632.png)

由此带来的问题是，用于获取传统参数数据的req.getParameter()方法无法处理JSON字符串。



异步请求的如下：

```
// 定义json对象
var user = {
                "username":"aaa",
                "hobbies":["hobby1","hobby2","hobby3"],
                "account":{"id":"X0012","pwd":"123"}
            };
// Jquery发出异步post请求            
$.post("ajaxServlet", user); //回调函数和返回类型缺省
```



发送的JSON字符串如下

`username=aaa&hobbies%5B%5D=hobby1&hobbies%5B%5D=hobby2&hobbies%5B%5D=hobby3&account%5Bid%5D=X0012&account%5Bpwd%5D=123`



服务器需从请求中抽取出JsonStr:

`{"username":"aaa","hobbies":["hobby1","hobby2","hobby3"], "account":{"id":"X0012","pwd":"123"}}`



然而服务器如何将String解析为Java对象,就是这里讨论的内容。

---

## 解析器

作用

- 将JSON字符串的解析为特定语言可以处理的对象

- 将对象格式化为JSON字符串



在java中Jackson实现Java对象与JSON字符串之间的转换。

在浏览器端自动实现Json对象与Json字符串的转换。

---



## 获取请求中的JsonStr

虽然我们有了Jackson，可以将JsonStr解析为Java对象。

但是浏览器将JSON对象‘塞’进了请求，我们该怎么取回来？？



先抄了一段代码。

```JAVA
package util;

import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

/**
 * @author kkddyz
 * @Description 获取请求中的JSON字符串
 * @Date 2021/7/6 17:14
 */

public class GetRequestJsonUtils {
    public static String getRequestJsonStr(HttpServletRequest request) throws IOException {
        String json = getRequestJsonString(request);
        return json;
    }

    /***
     * 获取 request中的Json字符串
     * @param request
     * @return : <code>byte[]</code>
     * @throws IOException
     */
    public static String getRequestJsonString(HttpServletRequest request)
            throws IOException {
        String submitMehtod = request.getMethod();
        // GET
        if ("GET".equals(submitMehtod)) {
            return new String(request.getQueryString().getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8).replaceAll("%22", "\"");
            // POST
        } else {
            return getRequestPostStr(request);
        }
    }

    /**
     * 描述:获取 post 请求的 byte[] 数组
     * <pre>
     * 举例：
     * </pre>
     *
     * @param request
     * @return
     * @throws IOException
     */
    public static byte[] getRequestPostBytes(HttpServletRequest request)
            throws IOException {
        int contentLength = request.getContentLength();
        if (contentLength < 0) {
            return null;
        }
        byte[] buffer = new byte[contentLength];
        for (int i = 0; i < contentLength; ) {

            int readlen = request.getInputStream().read(buffer, i,
                    contentLength - i);
            if (readlen == -1) {
                break;
            }
            i += readlen;
        }
        return buffer;
    }

    /**
     * 描述:获取 post 请求内容
     * <pre>
     * 举例：
     * </pre>
     *
     * @param request
     * @return
     * @throws IOException
     */
    public static String getRequestPostStr(HttpServletRequest request)
            throws IOException {
        byte[] buffer = getRequestPostBytes(request);
        String charEncoding = request.getCharacterEncoding();
        if (charEncoding == null) {
            charEncoding = "UTF-8";
        }
        return new String(buffer, charEncoding);
    }
}



```



---

## Jackson

### Maven依赖

```xml
 <!--Jackson required包-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.8.3</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.8.3</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.8.3</version>
        </dependency>
```

---

### ObjectMapper

- Jackson核心对象 ： 实现jackStr和java对象的转换



#### Java Object -> JsonStr

1. `String jsonStr = mapper.writeValueAsString(new Person() )` ： 将Person对象转化为JsonStr
2. `mapper.writeValue(arg,person);` :   将Person对象转化为JsonStr， 并流出内存；
   - arg代表输出的路径，可选参数有：
     - File 
     - Writer
     - OutputStream

---

##### Demo

```java
    @Test
    public void test1(){
        // 1. 创建java对象
        Person person = new Person();
        person.setName("张三");
        person.setAge(18);

        // 2. 创建Json核心对象 (ObjectMapper对象是定义将Object封装为目标的规则的对象)
        ObjectMapper mapper = new ObjectMapper();

        // 3. 转换 : java对象 -> json字符串 -> 流
        /*
            1. writeValue(arg,Obj) : 将obj写入目标()
                arg有很多种重载方式
                    1. File 写入文件
                    2. Writer  写入字符输出流
                    3. OutputStream 写入字节输出流

            2. writeValueAsString() 返回代表json对象的字符串
         */
        String jsonStr = "";
        try {
             jsonStr = mapper.writeValueAsString(person);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }

        System.out.println(jsonStr); // {"name":"张三","age":18}


        // 4. 流入文件
        try {
            // 相对路径从当前模块的根开始
            mapper.writeValue(new File("json.txt"),person);
        } catch (IOException e) {
            e.printStackTrace(); // 文件不存在
        }
    }
```

json.txt中保存的内容：`{"name":"张三","age":18}`

---

#### JsonStr -> Java Object

- `mapper.readValue(jsonStr.Object.class)`; 将JsonStr解析为对象 

  ```JAVA
  
  // 
  String jsonStr =
  	"{\"name\":\"张三\",\"age\":18,\"birthday\":\"2021-05-29 20:09:33\"}";
  
  try {
  	Person person = new ObjectMapper().readValue(jsonStr, Person.class);
  	System.out.println(person);
  } catch (IOException e) {
  	e.printStackTrace();
  }
      
  ```

  





---

### 注解 

> 注意我们在讨论的是注解，这两个注解的作用对象都是 field(成员变量)
>
> 那么作用的结果一定是对 （Object format的 ）JsonStr  产生影响

1. `@JsonIgnore` : 排除属性 

   - 被排出的属性不会写入JsonStr中

2. `@JsonFormat`: 时间格式化注解

   ```java
   // 在Person类中注解birthday format的格式及时区
   @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") 
   private Date birthday;
   
   // 测试方法
     public void test2(){
           // 1. 创建带有Date : birthday属性的对象
           Person person = new Person();
           person.setName("张三");
           person.setAge(18);
           person.setBirthday(new Date());
   
   
           // 2.创建解析器
           ObjectMapper mapper = new ObjectMapper();
   
           // 3. 获取jsonStr
           try {
               System.out.println(mapper.writeValueAsString(person));
           } catch (JsonProcessingException e) {
               e.printStackTrace();
           }
       }
   ```

   <font color="yellow">最后的结果：</font>

   ```TXT
    默认获取JsonStr      {"name":"张三","age":18,"birthday":1622207144775}
    加上JsonFormat注解后 {"name":"张三","age":18,"birthday":"2021-07-06 19:54:20"}
   ```
   
   > 这个`@JsonFormat`实际名字应该是：`@JsonDateFormat`
   >
   > 与`SimpleDateFormat`的作用是一致的。

---

# 案例：验证用户名是否存在



