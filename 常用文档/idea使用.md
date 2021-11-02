













# IDEA个人手册



### Maven项目resource加入classPath

一定把项目的packing标签设为jar 不然traget目录里没有resource(注意resource marked as root resource)

### 一运行idea就弹出 edit configruation

最下面 第一个选项不勾 第二个勾起来。

## 常见问题

1. src下新建package，package只是普通目录
   - 原因：package包名非法的。

- java命名规范

![img](https://user-gold-cdn.xitu.io/2019/12/17/16f118e59b84b888?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

多个评级的单词用_ 分隔。

---

2. 运行报错非法字符

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200718203817.png" alt="image-20200718203816513" align="left" />

**原因**

用Windows记事本打开并修改.java文件保存后重新编译运行项目；

<font color="blue">Windows记事本在修改UTF-8文件时会在文件开头添加BOM，会导致出现IDEA读取.java文件是无法识别的字符。</font>

**解决办法**

在编辑器IDEA中将文件编码更改为UTF-16，再改回UTF-8即可，其实就相当于刷新了一下文件编码。
当然你也可以使用Notepad或者Sublime对文件编码重新定义，这样也行。

**补充**

- 什么是BOM头？？

Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符即BOM头(在utf-8编码文件中BOM在文件头部，占用三个字节，用来标识该文件属于utf-8编码);

 



### Tomcat不能运行

`Cannot start compilation: the output path is not specified for module "". Specify the out`

原因其实是创建的空项目时没有out路径。

https://blog.csdn.net/zZ_life/article/details/51318306

---

## IDEA收藏夹 

使用 `Alt + 2` 可以快速打开 **Favorites** 列表。

>  上面说的标签以及断点会自动加入到 **Favorites**中。

## IDEA标签功能

首先介绍的是 IDEA 的书签功能，可以通过书签快速跳转到相应的源码。

IDEA 书签分为两种

- 匿名书签，可以生成无数个，使用快捷键 `F11` 快速生成。

- 标记书签，可以用数字或字母标记书签，总共只能生成 10 个数字以及 26 个字母的标记书签，使用快捷键 `Ctrl+F11`生成。

  > 另外使用数字标记的标签，可以使用 `Ctrl + 数字键` 跳转到相应标签，快速查看源码。

管理方式  `Shift + F11`
可以打开书签管理窗口，在这里可以删除标签，排序标签，以及给标签添加简单的解释。

## IDEA 热键

`ctrl+q` 查看说明

`Ctrl + Y` 删除光标所在行  

`Ctrl + Shift + /`  多行注释

`Ctrl + Shift + ⬆/⬇`  上下移动光标所在代码行，与其他行交换位置`。`

`Alt + Ins` 自动生成方法： constructor() toString() ;

`shift+F6` 将光标放在批量修改的变量上，连按两次。S

## IDEA版本控制

[将idea项目提交到Github](https://blog.csdn.net/Luck_ZZ/article/details/79541093)

所以idea和ssh没有半毛钱关系

## 类注释与方法注释

- [参考1](https://blog.csdn.net/qq_34581118/article/details/78409782)
- [参考2](https://www.jianshu.com/p/09139b425cc3)

在new -- java class -- java 之后生成的class会带有注释

livetemplet变量 是$date$,作为模板快速插入

 File and Code Templates 是${date} 是预设模板；

[idea手册](https://www.w3cschool.cn/intellij_idea_doc/intellij_idea_doc-69pu2eai.html)

## 快捷输入

[live tamplates 学习笔记](https://blog.csdn.net/cgl125167016/article/details/78732957)

定位光标位置  ( 现在这个符号没用了不知道为啥)

- `$END$`是一个特殊的预定义变量，表示光标最后跳转的位置。每个变量的位置都可以跳转过去。

## debug



https://zhuanlan.zhihu.com/p/95442097

## Javap

### 配置

- Program .exe路径
- Argu 参数
- Working directory:执行cmd的目录

1. IDEA失败

![image-20201101162453957](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201101162454.png)

```
# 输出cmd 发现path不对 out压根没有_enum.Session
D:\Java\AdoptOpenJDK\jdk-11.0.8-hotspot\bin\javap.exe -c _enum.Season
```

2. cmd下javap失败

```
F:\java-workspace\JavaClassDemo\out\production\_enum>java -p MyEnum.class
Error occurred during initialization of boot layer
java.lang.module.FindException: Module format not recognized: MyEnum.class
```

3. cmd下javap成功

   `javap -c MyEnum.class`

   - 步骤2写错了

- 与步骤一比较发现没有.class后缀

4. 修改Argus 原来还有文件名
- ![image-20201101164444351](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201101164444.png)

```
D:\Java\AdoptOpenJDK\jdk-11.0.8-hotspot\bin\javap.exe -c _enum.MyEnum.class
```

发现以全类名作为javap参数

5. 参考标准配置 

   ![image-20201101165754580](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201101165754.png)

![image-20201101165722855](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201101165722.png)

问题在于修改了outputpath，







---

每次都要配maven 

每一都要配 language level

修改 $outputpath$!!! 需要重启



---

