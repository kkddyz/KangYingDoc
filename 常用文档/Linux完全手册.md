---


## VMware Ubuntu环境搭建 

### Linux网络设置

#### 桥接模式(bridged)

*在这种模式下，VMWare虚拟出来的操作系统就像是局域网中的一台独立的主机*

需要进行手动配置。

那么配置那些内容呢？？

##### 虚拟网卡

bridged模式下的VMnet0虚拟网络

---

#### NAT(网络地址转换模式)

虚拟系统借助NAT(网络地址转换)功能，通过宿主机器所在的网络来访问公网。

*也就是说，使用NAT模式可以实现在虚拟 系统里访问互联网。*

NAT模式下的虚拟系统的TCP/IP配置信息是由VMnet8(NAT)虚拟网络的DHCP服务器提供的，无法进行手工修改.

DHCP服务器自动分配IP地址。

虚拟系统也就无法和本局域网中的其他真实主机进行通讯。

*采用NAT模式最大的优势是虚拟系统接入互联网非常简单，你不需要进行任何其他的配置，只需要宿主机器能访问互联网即可。*

##### 虚拟网卡

NAT模式下的VMnet8虚拟网络

---

### 修改软件源

Ubuntu默认的软件源是国外的，安装软件会比较慢，我们把他修改为国内阿里云的软件源。
打开软件和更新。

![img](https://pic2.zhimg.com/80/v2-20bc92f672e84fc3e5bd5cac0cedeff9_1440w.jpg)

选择【软件更新】

![img](https://pic3.zhimg.com/80/v2-85d536f58d341d3095b7afcf59be75d2_1440w.jpg)

点击【下载自】选择【其他服务器】

![img](https://pic4.zhimg.com/80/v2-9375f985f858f1694daa1530636fac0b_1440w.jpg)

点击【关闭】保存即可，他会提示让你刷新列表，刷新一遍就行啦。



---

### APT

通过apt安装chrome网太慢了

#### 介绍

apt — advanced package tools. 

It installs package dependencies for us, makes it easier for us to find packages that we can install, cleans up packages we don't need, and more. 

简单说就是 easy install



##### 修改sources.list

package是通过/etc/apt/sources.list 的URL来寻找。但是内置的太慢了，用的英国repository,需改成国内的。

`sudo mv /etc/apt/sources.list sources.list_backup` 

完了原来的位置会默认生成一个sources.lis，但是里面没有内容，所以apt是找不到chrome的



使用nano编辑文档 

```
nano /etc/apt/sources.list
复制内容
Crtl+o保存
```

update package repositories 

`sudo apt-get update`

换上清华阿里源，起飞

```
#添加阿里源
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
#添加清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.
```



----

### wget 下载chrome

Chrome不是一个开源的浏览器，不包含在标准的Ubuntu软件源中

`sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/`

使用wget下载(默认下载路径就是home)

`wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb`

amd64 指64位处理器架构 .deb 是Debian系统安装包类型

`sudo apt isntall ./google-chrome-stable_current_amd64.deb`



安装过程中Google官方软件源被添加到 sources.list.d/google-chrome.list

当新版本发布时自动升级。

----

##### sources.list.d 的作用

The function of the `/etc/apt/sources.list.d` directory is as follows:

Using the directory you can easily add new repositories without the need to edit the central `/etc/apt/sources.list` file. I.e. you can just put a file with a unique name and the same format as `/etc/apt/sources.list` into this folder and it is used by apt.

In order to remove this source again you can just remove that specific file without the need for handling side effects, parsing or mangling with `/etc/apt/sources.list`. It's mainly for scripts or other packages to put their repositories there automatically - if you manually add repositories you could add them to `/etc/apt/sources.list` manually.



##### gpg: no valid OpenPGP data found.

APT is complaining about a missing GPG key which you have to manually import before you can use your newly added repository (GPG verifies all data cryptographically and needs the public keys of the owners for this).



### 安装Clion 

官网下载安装包

CLion-2020.2.4.tar.gz 解压 Downloads/Software 

参考安装说明Install-Linux-tar.txt 运行`./clion.sh` 安装



#### 配置clion环境变量PATH

`cat etc/environment` -- 全局设置

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
```

我安装的clion是在home下的路径应该是属于用户的 

echo "export PATH=$PATH:/path/to/your/directory"

 `cat ~/.bash_profile ` -- 查看用户配置

 ```bash
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/kkddyz/Software/clion-2020.2.4/bin
/home/kkddyz/Software/clion-2020.2.4/bin #就是clion的path
 ```

`source ~/.bash_profile` --- 手动导入配置 



可是我根本找不到clion的执行文件

#### 配置Clion

![image-20201016142032347](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201016142032.png)

根据官方说明Ubuntu需要安装以下依赖。

默认的Ubuntu软件源包含了一个软件组"bulid-essential"

![image-20201016142508636](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201016142508.png)

但是安装的时候出现了问题 ： 

```
The following packages have unmet dependencies:
 gsettings-desktop-schemas : Breaks: mutter (< 3.31.4) but 3.28.4-0ubuntu18.04.2 is to be installed
E: Error, pkgProblemResolver::Resolve generated breaks, this may be caused by held packages.
```

包依赖损坏

apt-get remove mutter

apt-get autoremove





## 工具篇

### 文本编辑器

> vi (visual editor) 
>
> Linux系统内置的最基本的editor。
>
> 许多软件的编辑接口都会去调用vi
>
> vim是vi的升级版

---

#### Vim 

##### 命令行模式

- 复制删除

| cmd       | 含义                                   | 备注                               |
| --------- | -------------------------------------- | ---------------------------------- |
| 定位      |                                        |                                    |
| gg        | navigate to the first line of the file | 3gg navigate to the third line     |
| G         | navigate to the last line              |                                    |
| home      | beginning of the line                  | ^ 快捷键 shift+6 (home键 不是输入) |
| end       | end of the line                        | $ 快捷键Shift+4                    |
|           |                                        |                                    |
| 复制粘贴  |                                        |                                    |
| yy        | copy cursor line                       | 3yy copy 3line                     |
| p(小写)   | paste after cursor                     |                                    |
| P(大写)   | paste before cursor                    |                                    |
|           |                                        |                                    |
| 删除      |                                        |                                    |
| dd        | delete line                            | 3dd 删除三行                       |
| d^        | 删除光标之前                           | ^ 托字符                           |
| d$ \|\| G | 删除以后                               |                                    |
| dgg       | 删除之前                               | 包括光标                           |
| dG        | 删除光标以下行                         | 包括光标                           |
| x         | 删除光标后字符 3x 删除三个字符         | 相当于Windows的delete键            |
| X         |                                        | 相当当于backspace                  |
|           |                                        |                                    |
| u         | 撤销                                   |                                    |

##### 编辑模式

- 插入操作

| cmd  |              含义               | 备注 |
| :--- | :-----------------------------: | ---- |
| i    | insert 在光标所在字符前开始插入 |      |
| I    |           定位到行首            |      |
| a    |   after 定位到 cursor后面字符   |      |
| A    |           定位到行末            |      |
| o    |      other 另起下一行开始       |      |
| O    |           另起上一行            |      |
| S    |    删除光标所在行并开始插入     |      |
|      |                                 |      |
| Esc  |          退回命令模式           |      |

---

##### 底行模式

模式切换 : cmd模式按 ":" 

1. 保存和退出 

- 保存文件 `:w` ; 另存为 `:w file_name`
- 退出 `:q` ; 强制退出 `:q!`

- 保存退出 `:wq` ;强制保存退出 `:wq!`
- `:x`  <==> `:wq` 

2. 字符匹配

   1. 查找

   - cmd模式 : `/` + pattern | | n下一个 N上一个
   - 底行模式 `:/`

   2. 替换 

   - `%s/from/to` (默认替换每行第一个)

   - %s/from/to/g (g表示全部替换)

   - /gc (询问是否替换) 


3. 流操作 


   1. 读入：

      1. 

      -  :`r file` 默认光标所在行readI

   

      1. `:r filestream`

   2. 

   3. `:4r filestream` 从文件第四行开始读入

```
1. :set list 查看控制符号 比如 *$换行 ; ^I tap* 
2. a,b /save/to/path.txt a,b行另存文件

```




   1. 

   2. 

      

>--- 分隔符问题
>
>/ 作为分隔符也会出现在查找文本中。
>
>1. **自定义分隔符** `%s#/usr/bin#/usr/BIN#g` 
>2. **转义**`%s/\/usr\/bin/\/usr\/BIN#g`-- *这个看起来着实不方便*

>--- vim显示行号和缩进 
>
>sudo -s # 提权到root ~ <==> /
>
>echo "set number" >>  /etc/vim/vimrc # 添加到文件
>
>\# set ts=4 四格缩进
>
>



---

安装ssh

https://segmentfault.com/a/1190000022103074

运行 apt-get 时报如下错

```text
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```

解决方案:

```text
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
sudo rm /var/cache/apt/archives/lock
```



---

## 虚拟机重新起步

重新装Ubuntu，有点烦。

[教程](https://zhuanlan.zhihu.com/p/38797088)



---

[换源](https://zhuanlan.zhihu.com/p/61228593)