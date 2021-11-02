## 问题记录一：refusing to merge unrelated histories

##### 问题描述

在云端创建项目，有三个文件；

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210711102811.png" alt="image-20210711102810972" style="zoom:67%;" align="left"/>

在本地也有一次提交

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210711104239.png" alt="image-20210711104239359" style="zoom: 67%;" align="left"/>



idea使用gitee插件通过账号验证。



##### git pull origin master

```
F:\javaweb_workspace\univ-Resource_bank>git pull origin master
From https://gitee.com/kkddyz/univ-resource_bank

 * branch            master     -> FETCH_HEAD
   fatal: refusing to merge unrelated histories


```

提示不允许合并不相关的历史



##### 参阅资料

> ## The “fatal: refusing to merge unrelated histories” Git error
>
> The **“fatal: refusing to merge unrelated histories”** Git error occurs when two *unrelated* projects are merged (i.e., projects that are not aware of each other’s existence and have mismatching commit histories).
>
> ![svg viewer](https://www.educative.io/api/edpresso/shot/4755609222119424/image/5534126872461312)
>
> Consider the following two cases that throw this error:
>
> - You have cloned a project and, somehow, the `.git` directory got deleted or corrupted. This leads Git to be unaware of your local history and will, therefore, cause it to throw this error when you try to *push to* or *pull from* the remote repository.
> - <font color="red">You have created a new repository, added a few commits to it, and now you are trying to pull from a remote repository that already has some commits of its own. Git will also throw the error in this case, since it has no idea how the two projects are related.</font>
>
> ## Solution
>
> The error is resolved by toggling the *allow-unrelated-histories* switch. After a `git pull` or `git merge` command, add the following tag:
>
> ```git
> git pull origin master --allow-unrelated-histories
> ```
>
> More information can be found [here, ](https://github.com/git/git/blob/master/Documentation/RelNotes/2.9.0.txt#L58-L68) on Git’s official documentation.
>
> 



##### 发现原因

我在云端进行一次commit 又在本地进行commit(没有先pull)；

从云端fetch到副本后，git发现两个记录是没有关联的（相当于两个人独立记录提交到云端，这是很容易出现冲突的）因此git 拒绝合并

![img](https://pic4.zhimg.com/80/v2-ae4d52c15caae9b22fe6063fa96bbbcf_720w.jpg)





##### 解决办法 

```
git pull origin master --allow-unrelated-histories
```



---

## 问题记录二

##### 问题描述

在解决问题一后 

因为我觉的readme之类的文件没啥用，就直接在gitee中删除。

每次删除都产生了一次新的commit，我当时没在意。

后来我自己敲了一部分代码想提交到gitee 发现不可以push。

这时候我才想到因该在删除文件后先pull一下再增加新内容。





##### 问题模型

现在的git版本模型是这样的：

假设问题一解决后云端和本地版本都是 a

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210711210710.png" alt="image-20210711210710812" style="zoom:80%;" align="left"/>



我查看git提交记录 发现确实是这样 。

---

##### 报错信息

git push origin master

> F:\javaweb_workspace\univ-Resource_bank>git push origin master
> To https://gitee.com/kkddyz/univ-resource_bank
> ! [rejected]        master -> master (non-fast-forward)
> error: failed to push some refs to 'https://gitee.com/kkddyz/univ-resource_bank'
>
> hint: <font color="red">Updates were rejected because the tip of your current branch is behind its remote counterpart(对等部分)</font> . counterpartthe remote changes (e.g. 'git pull ...') before pushing again. <font color="gree">出现原因 在push前云端文件改变</font>
> See the 'Note about fast-forwards' in 'git push --help' for details.



git pull origin master

> F:\javaweb_workspace\univ-Resource_bank>git pull origin master
> From https://gitee.com/kkddyz/univ-resource_bank
>
> branch            master     -> FETCH_HEAD 合并时发现问题
>
> error: Your local changes to the following files would be overwritten by merge:
> README.en.md README.md



避免直接修改云端的数据：会导致平行分支



##### 问题分析

错误的产生是由于暂存区还有没有commit的文件，此时pull会导致这些没有commit的changes遗失，

所以git在merge时主动停止。

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210711221158.png" alt="image-20210711221158569" style="zoom:80%;" align="left"/>

----

我们发现本地的修改文件和远程仓库的是相互独立的可以直接合并 

但是现在工作区中在增加文件的提交之后 又发生一些改变



##### 处理方法

git commit -m"删除readme" 

然后就可以正常pull合并分支，然后在push

----

## 问题记录三: 

```
F:\JAVAEE_Workspace\learn_mybatis>git pull origin master
fatal: couldn't find remote ref master

F:\JAVAEE_Workspace\learn_mybatis>git push -u origin master
Enumerating objects: 86, done.
Counting objects: 100% (86/86), done.
Delta compression using up to 12 threads
Compressing objects: 100% (44/44), done.
Writing objects: 100% (86/86), 14.47 KiB | 1.03 MiB/s, done.
Total 86 (delta 6), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (6/6), done.
To github.com:kkddyz/learn-mybatis.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

猜测:在github刚创建一个项目的时候是没有任何分支的,自然不会有master.

确实没有,但是你push不就有了,

我重新创建一个test-push的仓库,使用 `git push origin master` 没有出问题.



所以,我认为这是个意外,做不研究,





## 问题记录4

#### 描述

提交的时候发现test文件没有提交,然后就以amend的方式提交(提交的时候重命名了);

这个文件是新建的remote是没有的.

尝试push

```
F:\JAVAEE_Workspace\learn_mybatis>git pull
error: Your local changes to the following files would be overwritten by merge:
test/TestMybatis.java
```



---

#### 分析

查看本地的提交记录

git log --pretty=oneline

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210828201300.png" alt="image-20210828201300284" style="zoom:80%;" />

查看github的commit

![image-20210828201448896](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210828201448.png)

查看idea的git图 

![image-20210828201609508](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210828201609.png)



git status 发现重命名的文件是new file

![image-20210828202529172](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210828202529.png)

原来之前的amend的commit没有自动add重命名的文件



把test提交之后  git push

```
F:\JAVAEE_Workspace\learn_mybatis>git push
To github.com:kkddyz/learn-mybatis.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'github.com:kkddyz/learn-mybatis.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```



先pull 

```
F:\JAVAEE_Workspace\learn_mybatis>git pull
Merge made by the 'recursive' strategy.
```

> **Recursive策略**
>
> Recursive策略是Git在对两个分支进行合并时所采用的默认策略，它只适用于两个分支之间的合并。因此，对于超过两个分支的合并，需要反复地进行两两合并，才能最终完成所有分支的合并（这也是Recursive名字的由来）。本质上，Recursive就是一种Three-Way Merge。它的特点在于，如果Git在寻找共同祖先时，在参与合并的两个分支上找到了不只一个满足条件的共同祖先，它会先对共同祖先进行合并，建立临时快照。然后，把临时产生的“虚拟祖先”作为合并依据，再对分支进行合并。
>
> 如下图所示，在对两个分支上的提交A和B进行合并时，我们发现了它们有两个共同祖先，分别是：ancestor0和ancestor1。这个时候，Recursive策略会对ancestor0和ancestor1进行合并，临时创建一个虚拟祖先：ancestor2。用ancestor2与A，B一起进行Three-Way Merge。
>
> <img src="https://morningspace.github.io/assets/images/lab/git/merge-stories-10.png" alt="img" style="zoom:67%;" />

然后push 就可以了.

---

#### 总结 

出现的诱因是没有将所有的修改都add.

其中的原理我不明白,

学到的教训是 git push出现问题 先看一下status,work tree clean后   先pull后push





## 使用ssh方式验证连接

> [教程](https://www.jianshu.com/p/dd3be8cb5b90)

使用非对称密码ssh来保证 远程仓库的安全。

当我们使用 pull,push 向git服务器发出读写请求时，会携带私钥生成的验证文件(就像文件上的印章)。

服务器 根据公钥 验证请求者 的 验证文件 ，如果可以识别说明公私钥是一对，可以访问。

如果无法验证，请求会被拒绝。

```
# 创建钥匙对
sh-keygen -t rsa -C "这里换上你的邮箱"

# 测试是否配置成功
ssh -T git@github.com 
```

#### 问题

本地存在一个ssh钥匙对,之前的仓库都可以正常连接github,现在新建的却不可以.

难道每个仓库都需要单独建立一个ssh??不应该呀

使用ssh测试命令

```
test-ssh>ssh -T git@github.com
git@github.com: Permission denied (publickey).
```

原因是那个是连接gitee创建的,原来的github的在上个月重装系统的时候就已经没了;

我是看那个ssh公钥的最后一次使用时间在9个月以前,才突然想到的,

## 创建分支关联

最近使用git pull的时候多次碰见下面的情况：

```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

git branch --set-upstream-to=origin/<branch> release
```

其实，输出的提示信息说的还是比较明白的。

使用git在本地新建一个分支后，需要做远程分支关联。如果没有关联，git会在下面的操作中提示你显示的添加关联。

关联目的是在执行git pull, git push操作时就不需要指定对应的远程分支，你只要没有显示指定，git pull的时候，就会提示你。

解决方法就是按照提示添加一下呗：

`git branch --set-upstream-to=origin/remote_branch  your_branch`
其中，origin/remote_branch是你本地分支对应的远程分支；your_branch是你当前的本地分支。

## 分支管理

### 分支创建

> 补充 HEAD 概念：
>
> HEAD是一个特殊指针，指向当前所在的本地分支；

**Git 是怎么创建新分支的呢？ 很简单，它只是为你创建了一个可以移动的新的指针。** 

比如，创建一个 dev分支， 你需要使用 `git branch` 命令：

```console
$ git branch dev
```

这会在当前所在的提交对象上创建一个dev指针,如下图

![image-20201204191346293](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201204191346.png) 

 因为 `git branch` 命令仅仅 **创建** 一个新分支，我们需要使用`git checkout` 切换分支dev。

在几次commit之后，情况会变成这样：

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201204193723.png" alt="image-20201204193723061" style="zoom:33%;" align="left"/>



### 分支切换

>```
>$ git checkout master
>```
>
>这条命令做了两件事。 一是使 HEAD 指回 `master` 分支，二是将工作目录恢复成 `master` 分支所指向的快照内容。 也就是说，你现在做修改的话，项目将始于一个较旧的版本。 
>
>本质上来讲，这就是忽略 `dev` 分支所做的修改，以便于向另一个方向进行开发。

这时结构就形成了一棵树，如下图：

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201204193856.png" alt="image-20201204193856746" style="zoom: 33%;" align="left"/>



这说明了分支的特点，实现*并行开发*。

> **命令简写**
>
> ```console
> $ git checkout -b dev
> Switched to a new branch "dev"
> ```
>
> 它是下面两条命令的简写：
>
> ```console
> $ git branch dev
> $ git checkout dev
> ```



### 分支合并

##### 简单合并(fast-forward)

现在我们已经完成了dev分支的开发任务C7作为最终版本提交，现在需要将其合并回master；

> **合并分支**
>
> ```
> $ git checkout master
> $ git merge dev
> ```

示意图

![image-20201204202228694](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201204202228.png)



> **fast-forward**
>
> 在合并的时候，出现提示 `fast-forward`。
>
> 由于你想要合并的分支 `dev` 所指向的提交 `C6` 是你所在的提交 `C3` 的直接后继.因此 Git 会直接将指针向前移动。
>
> 换句话说，当你试图合并两个分支时， 如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。



##### 三相合并

![继续在 `iss53` 分支上的工作。](https://git-scm.com/book/en/v2/images/basic-branching-6.png)

<font color="green">问题出现的原因是 ：在开发iss53分支时C2出现了紧急问题，我们通过创建新分支hotfix修复并通过fastforward合并到C4。</font>

<font color="green">我们面临的问题是 ：将两个不同分支合并到一个公共祖先的过程中可能会出现对同一个文件不同的修改，那么应该保留哪一个呢？？</font>



和之前将分支指针向前推进所不同的是，Git 将此次三方合并的结果**做了一个新的快照**并且自动创建一个新的提交指向它。 这被称作一次合并提交。

![一个合并提交。](https://git-scm.com/book/en/v2/images/basic-merging-2.png)

既然你的修改已经合并进来了，就不再需要 `iss53` 分支了。 现在你可以在任务追踪系统中关闭此项任务，并删除这个分支。

> 删除分支：
>
> ```console
> $ git branch -d iss53
> ```



##### 遇到冲突时的分支合并

如果你对 #53 问题的修改和有关 `hotfix` 分支的修改都涉及到同一个文件的同一处，在合并它们的时候就会产生合并冲突：

```console
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

此时 Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 `git status` 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件：

```console
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。 Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。 出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

```html
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

这表示 `HEAD` 所指示的版本（也就是你的 `master` 分支所在的位置，因为你在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（`=======` 的上半部分），而 `iss53` 分支所指示的版本在 `=======` 的下半部分。 为了解决冲突，你必须选择使用由 `=======` 分割的两部分中的一个，或者你也可以自行合并这些内容。 例如，你可以通过把这段内容换成下面的样子来解决冲突：

```html
<div id="footer">
please contact us at email.support@github.com
</div>
```

上述的冲突解决方案仅保留了其中一个分支的修改，并且 `<<<<<<<` , `=======` , 和 `>>>>>>>` 这些行被完全删除了。 在你解决了所有文件里的冲突之后，对每个文件使用 `git add` 命令来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

---

#### 冲突合并demo分析

![image-20210715231648427](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210715231648.png)

基于相同的html文件，GCY对url接口的servlet路径做了修改，DJH修改了样式部分。即我的commit为C2; 

GCY与DJH在两个分支上并行开发，且不是fast-forward;

在GCY提交到gitee之后，丁不可以直接提交，因为C1,C3在三项合并时存在冲突。

DJH需要先将C2 fecth到本地，将C1,C2 merge。

GIt auto-merge 是将冲突的部分同时写入文件，DJH需要手动选择冲突部分的代码，保留正确版本。

合并成功后就可以得到C3.由于C3是C2的后继节点，GCY只需要进行fast-forward就可以实现C2 -> C3;



---

## restore 

####  撤销对文件的修改

--worktree 标志撤销的对象是工作区

```
  命令：git restore --worktree <path>
  举例：git restore --worktree test2.txt
  tips：如果暂存区有该文件的修改，恢复到和暂存区一致；如果暂存区没有该文件，会将工作区的文件恢复到和最近的提交的一致。–worktree 也可以省掉，即：git restore <path>
  
   命令：git restore .
```

 

#### 取消暂存的文件

-- staged表示撤销的对象是暂存区

```
命令：git restore --staged <path>
  举例：git restore --staged test2.txt

  举例2：git restore --staged ‘*.txt’
  tips：git restore 撤销暂存区某些文件的修改，将这些文件恢复到工作区去
```

#### 切换到上个 commit 版本

```
命令：git restore --source=HEAD~1 .
```

将工作区内容切换到某个版本库去

----

#### 测试

> 前提：在模型图中只存在一个文件对象 

我创建一个test目录测试restore命令  先创建第一个文件a.txt写入第一行内容。

这时的状态图如下：标记为状态 S1

![image-20210712143127623](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712143127.png)

使用status验证

```
E:\桌面\test>git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

---

使用add追踪新文件，并加入到暂存区。标记为状态S2

![image-20210712143649311](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712143649.png)

此时可以使用 `git rm --cached a.txt` 取消追踪并移出暂存区(状态改变 S2 -> S1)

```
E:\桌面\test>git rm --cached a.txt
rm 'a.txt'
```

---

S2状态下，修改工作区文件 标记为S3 	

![image-20210712144445672](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712144445.png)

```
E:\桌面\test>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt
```

---

使用restore (从暂存区或者版本库去寻找a.txt的最新版本 覆盖到工作区) 状态变化S3 -> S2

```
E:\桌面\test>git restore a.txt
```

---

在S3的状态下，使用restore恢复到s2(不可以在编辑器中手动将修改为S2的内容)

增加一行错误数据 ，并更新到暂存区。状态标记为S4

![image-20210712152749170](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712152749.png)

```
git restore --staged a.txt
fatal: could not resolve HEAD
```

我猜想restore命令本质是恢复(重写)而不是删除。

这个命令会从版本库中 查找 a.txt的最新版本然后覆盖暂存区原来的文件。

将这个错误标记为E1 

---

验证E1 在 S3的状态下。更新暂存区，并提交。标记为状态5

![image-20210712155204423](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712155204.png)

添加一行错误信息，并且add 

```
E:\桌面\test>git restore --staged a.txt

E:\桌面\test>git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

![image-20210712155818228](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712155818.png)



与rm --cache 不同 restore只是移除暂存区的文件，但是并未取消对文件的追踪。

---

E1错误的发生在文件未被commit原因不知。