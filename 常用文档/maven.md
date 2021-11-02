## Maven

### 介绍  

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200717124140.png" alt="image-20200717124140484"  align="left"/>

```
关于打包 ：通常指将编译得到class文件以包层次压缩为.jar文件
步骤	  1.将source与class文件分离
			因为执行不需要用到source,且开发者也不希望用户可以查看源文件；
            所以jar包里只包含可执行的java文件
        2.加入manifest.txt文件 :包含main主类的信息
        	只有这样，jvm才能找到app的入口；
        3.jar包必须有包结构来防止class文件名冲突
        	最后打包是将com目录以下放入jar包中
																			*** 详见headfirst java 11 发布程序 
```

### 作用

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200717135624.png" alt="image-20200717135624386" style="zoom:80%;" align="left"/>

读读就好没啥意思；

其中最主要就是依赖管理；

### 1. 安装maven

- 下载压缩包解压(解压就可以用了)
- 环境变量
  - 1. MAVEN_HOME maven所在文件夹
    2. path : %MAVEN_HOME%\bin
- 配置文件 ./conf/setting.xml

### 2. maven仓库

maven的通过”<dependency>标签“为项目引入依赖，而不是将依赖直接放在项目中；

换而言之，依赖与项目的代码分离，这种关系很像css的link标签；

当别人拿到项目运行时，根据maven依赖下载对应jar包



![img](https://raw.githubusercontent.com/kkddyz/photos/master/img/批注 2020-04-30 164038.png)

![](https://i.loli.net/2020/04/30/TMpGnxjoH4gF2B6.png)

### 3. 配置本地仓库

`<localRepository>F:\Maven_Repository</localRepository>`

### Maven标准目录结构

![](https://raw.githubusercontent.com/kkddyz/photos/master/img/QQ截图20200430165352.png)

src/main/webapp 如果是web项目的话



### maven命令

1. mvn compile -- 在根目录下生成target文件夹
   1. target下class是src/main/java 对应的字节码文件夹

2. mvn clean -- 删除targett目录
   - 原因：不同开发环境下源码的编译结果不一样

3. mvn test -- 编译测试代码

4. mvn package -- 打包
5. mvn install -- 把war包加到本地仓库

后面的命令执行时会确保之前的命令也按顺序执行了

![img](https://raw.githubusercontent.com/kkddyz/photos/master/img/QQ截图20200430200920.png)

<font color="blue">这些也组成了maven的生命周期</font>

> 注：clean不会从本地仓库删除jar包

### POM

- 模型对应着pom.xml文件的标签元素 	

![QQ截图20200430203814](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200704144137.png)





### IDEA集成Maven

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200717183640.png" alt="image-20200717183639594" align="left" />

### 创建Maven项目

#### Archetype是什么

- archetype 骨架

`简单的说，Archetype是Maven工程的模板工具包。一个Archetype定义了要做的相同类型事情的初始样式或模型。这个名称给我们提供来了一个一致的生成Maven工程的方式。Archetype会帮助作者给用户创建Maven工程模板，并给用户提供生成相关工程模板版本的参数化方法`

详见：https://www.cnblogs.com/wkrbky/p/6358485.html



- 选择maven-archetype-quickstar对应普通java工程

- -DarchetypeCatalog=internal (VM option填上)



#### 创建Mavenweb项目



- maven-archetype-webapp

----

#### 报错：

`问题1：Caused by: java.net.ConnectException: Connection refused: connect`

最终发现应该是IDEA版本和Maven版本不兼容的问题,一开始使用的是 
IntelliJ IDEA 2019.2.2 (Ultimate Edition)

`问题2 : Cannot resolve plugin org.apache.maven.plugins:maven-clean-plugin:2.5`

[链接](https://blog.csdn.net/hnu_zzt/article/details/97767545)

----

----



## 配置国内镜像

> [官网setting教程](http://maven.apache.org/settings.html)





## Maven Runner乱码

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210710115259.png" alt="image-20210710115259298" style="zoom:50%;" align="left" />

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210710115405.png" alt="image-20210710115405738" style="zoom:50%;" align="left"/>



![image-20210710115724437](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210710115724.png)



> https://blog.csdn.net/yeliping2011/article/details/41848161
