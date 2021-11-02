## 正则表达式

## 第一次学习

> python--北大--MOOC

### 概念引入

1. 我们为什么需要正则表达式?

   以这组字符串为例：'PN','PYN','PYTN,'PYTHN','PYTHON'; 它的描述方式为**一一列举**

   

   通过观察我们可以发现一些**特征(规则)**：*都以P开始，N结束。*

   如果用语言描述就是，*一组以PN为首尾中间分别'',‘Y’,‘YT‘,’YTH‘,’YTHO‘的一组字符串。*

   显然像这样用规则(特征)描述一组字符串相比于一一列举更加**简洁**；

   

​		但是，语言描述本身并不简介，显然我们需要一套表达方式来代替这种语言描述。

​		于是就产生了正则表达式，这样一套表示**基于特征**的表示体系

​		这里的正则表达式是——P(Y|YT|YTH|YTHN|)?N ;

​	

 2. 总结一下上文 ： 

    正则表达式是**基于规则描述**一组具有**特征**的字符串的**简洁**的表达方式。

    关键词显然： **简洁与特征**;

    简洁的原因是有限的特征可以组合出相当庞大的字符串对象；



3. 主要应用：文本处理 具体点：匹配字符串(查找字符)；

   根据特征可以产生对应的字符串组，反之也可以根据规则判断字符串是否符合规则；



4. 补充：正则表达式的编译

   1. 正则表达式是一种特征,但是以字符串的形式出现;

   2. 程序不会对 身为 字符串的“RegExp”另眼相看，需要程序员将其转换；

   3. 将表示正则表达式的字符串转换成相应的特征(对象) —— 这个过程称为编译；

   4. 不同语言中的编译方式不同；



### 语法

- RegExp规定了哪些特征(思考字符串之间会具有哪些共同特征？)



我们学习的其实是特征的种类与符号表示;

这些符号被称为操作符。



#### 常见操作符

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200704173649.png" alt="image-20200704173649502" style="zoom: 100%;" align="left"/>

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200704173649.png" alt="image-20200704173649502" style="zoom: 100%;" align="left"/>



研究这些符号，我们可以对特征进行总结: (? 作为通配符表示任意的字符或者字符串)

1. 以 ？开头/结尾
2. 对 ？指定范围  
3. 对 ？扩展N次；

还有一些特殊符号 . \d \w 表示数字或者字母；(以类别作为特征)



特征的设计思想：通过以字符为最小单位 ， 按顺序(字符数组的下标0-length)筛选(组合)字符;



#### 常见的特征表达式

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200704175340.png" alt="image-20200704175340809" style="zoom:80%;"  align="left"/>

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200704175735.png" alt="image-20200704175735860" style="zoom:80%;" align="left"/>







### 问题引入

> 
>
> 原始需求:
> 解析 complex.format()得到的formatted:String,如: "(1,2i)"
>
> 难点在于匹配对象是模式而非具体，得到 [int real=1 ; int real = 2] 这两个complex对象的属性
>
> 因此String提供的提取方法不起作用；
>
> 符合需求的工具是 **Java Regular Expression**



### 截取字符串(无效方法)

> 补充String提取字符串的常用方法
>
> 
>
> <img src="https://img-blog.csdn.net/20170809090841097?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveHVlaHl1bnl1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="img" style="zoom:80%;" align="left"/>
>
> <img src="https://img-blog.csdn.net/20170809090908389" alt="img" style="zoom:80%;" align = "left"/>





## 第二次学习

> 看的哔哩哔哩第一个视频





#### 普通字符

- 表示本身

简单的转义字符

![image-20201127164702982](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210324221335.png)

> 转义字符是指用\修饰的字符 
>
> 而原本的字符在正则表达式中有着特殊的含义。



#### 标准字符集合

- 表示模式 
- 大写表示取反
  - 比如 \d 匹配数字 \D匹配非数字

| regx | 匹配对象       | 说明                                    |
| ---- | -------------- | --------------------------------------- |
| \d   | 数字           | null                                    |
| \w   | 字母数字下划线 | 合法用户名的组成字符                    |
| \s   | 空白字符       | line break ；space ； tab               |
| .    | 任意字符       | 除了换行符 如果要匹配换行符一般用[\s\S] |



#### 自定义字符集合

| regx    | 匹配对象        | 说明                                         |
| ------- | --------------- | -------------------------------------------- |
| [abc]   | a \|\| b \|\| c | 匹配a或b或c                                  |
| \[^abc] | 匹配abc以外字符 | 通过^取反                                    |
| [0-a]   | 匹配连续字符    | 字符序列的顺序是 0-9 A-Z a-z (按ascii码升序) |

- 注意之前提到的转义字符在[]在没有歧义的情况下不需要加\   



#### 量词

| regex   | 匹配对象      | 说明                                                      |      |
| ------- | ------------- | --------------------------------------------------------- | ---- |
| \d{6}   | 连续六个数字  | \d\d{6}匹配7位 (\d\d){6} 匹配12位感觉像乘法优先级比加法高 |      |
| \d{m,n} | 匹配n,m位数字 | 贪婪模式: 匹配的越多越好                                  |      |
| \d{m,}  |               | 至少重复m次                                               |      |
| \d?     |               | 等价于 \d{0,1}                                            |      |
| \d+     |               | 等价于 \d{1,}                                             |      |
| \d*     |               | 等价于 \d{0,}                                             |      |



#### 字符边界

- 表示的不是字符而是边界,对匹配字符的位置位置进行限定(满足规则的字符串还必须出现在指定的位置)
- 由于边界和光标位置一样处在两个字符之间，因此是"零宽"的。

| regex | 匹配对象        | 说明                                                         |
| ----- | --------------- | ------------------------------------------------------------ |
| ^abc  | 字符串开头的abc | ^匹配字符串开始的位置                                        |
| abc$  | 字符串结尾的abc | $匹配字符串的结束位置                                        |
| \babc |                 | 匹配一个位置，前面的字符和后面的字符不全是\w 不是\w可以是什么呢？ 可以是空白字符和@#%$这些不构成单词的字符 |



#### 匹配模式

- IGNORECASE 忽略大小写 
- SINGLELINE 单行模式  
  - 将整个文本看作一个字符串，只有一个开头和结尾
  - 这时 "." 可以匹配"\n"
- MUTILINE 多行模式 
  - 每行都是一个字符串，都有开头和结尾。
  - 该模式下使用 \A 匹配最开始位置 \Z匹配最末尾



#### 选择符和分组

| regx                                                         | 匹配对象                                                     | 说明 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| \|                                                           | 配对对象满足左表达式或右表达式                               |      |
| ()                                                             捕获组 | 1. 作为整体被量词修饰                                                                                                                                              2. 取匹配结果的时候，括号中的表达式匹配到的内容可以被单独拿到                                                                                                                                           3. 每一对括号分配编号从1开始                                                                                                                      4.group(0)表示的是符合所有分组的匹配对象 |      |
| (?:)                                 非捕获      组          | 捕获组会自动将捕获到的字符放入内存 ，但是有时候我们只是需要使用括号来组织表达式结构(把某部分看作整体)，而不会用到反向应用，这时候就需要使用？声明。 |      |



##### 反向引用(\nnn)

- 对已捕获的分组字符串进行引用
- 他表示的是普通字符而不是模式

例如：

```
pattren : [a-z]{2}\1 
string : asdasd gogo 
// 匹配结果 gogo \1以引用的方式
```







#### 预搜索(零宽断言)

| regx                      | 匹配对象                | 说明 |
| ------------------------- | ----------------------- | ---- |
| (?=exp)  `([a-z]+)(?=ing) | 断言此位置后面一定是exp |      |
| (?!exp)                   | 断言此位置后面不是exp   |      |
| (?<=exp)                  | 断言此位置前面一定是exp |      |
| (?<!exp)                  | 断言此位置前面不是exp   |      |



#### Java语法

- Pattern :
  - 正则表达式的编译表示形式
  - Pattern p = new Pattren.compile(r,int);//建立正则表达式，并启用相应的模式
- Matcher
  - 通过解释Pattern对character sequence 执行匹配操作的引擎
  - Macher m = p.matcher(str); // str 匹配字符串

```JAVA
package regex;

import com.sun.org.apache.xerces.internal.impl.xpath.regex.Match;

import java.util.Arrays;
import java.util.regex.Matcher;
import java.util.regex.Pattern;


public class Demo1 {
    public static void main(String[] args) {


        //pattern 定义匹配模式
        Pattern p = Pattern.compile("([0-9]+)(\\.)([0-9]{1,2})");// 假设拿到的数字保留1-2位小数

        String str = "(23.00,3.5i)";
        // matcher封装匹配结果
        Matcher m = p.matcher(str);

        /**
         * 打印匹配到的字符串
         * 1. find() 会匹配下一个并保存结果，返回值类型 boolean
         * 2. group() 返回当前匹配结果
         */

        while (m.find()){
            System.out.println(m.group());
        }

        /**
         * 替换匹配到的字符串
         */
        String newStr = m.replaceAll("###");
        System.out.println(newStr);

        /**
         * 字符串分割
         */
        String regex = "[0-9]+"; // 匹配数字
        Pattern p1 = Pattern.compile(regex);
        String str1 = "aaa222bbb342ccc23424dde";
        String[] arrays = str1.split("[0-9]+"); // 以字符串作为分隔符
        System.out.println(Arrays.toString(arrays));

    }
}

```

---

