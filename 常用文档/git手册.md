	Git

- [Git Book](https://git-scm.com/book/zh/) 比较全
- [科普文章：git的底层数据结构与分支模型](https://zhuanlan.zhihu.com/p/386919315?utm_source=qq&utm_medium=social&utm_oi=1036389315084918784)
- [菜鸟教程](https://www.runoob.com/git/git-server.html) 比较精简

### 0.安装 

一路next就好

### 1.1Git配置

#### **config文件**

git config存储在三个不同的位置：

1. `$Git_HOME$/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 `git config` 时带上 `--system` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 你可以传递 `--global` 选项让 Git 读写此文件，这会对你系统上 **所有** 的仓库生效。
3. 当前使用仓库的 Git 目录中的 `config` 文件（即 `.git/config`）：针对该仓库。 你可以传递 `--local` 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）

每一个级别会覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。

---

##### **检查配置信息**

1. 查看所有的配置以及它们所在的文件 （--show-orgin多了一行所在文件）

`git config --list --show-origin`

![image-20201102205513404](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201102205513.png)

> 你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：`/etc/gitconfig` 与 `~/.gitconfig`）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

2. 你可以通过输入 `git config <key>`： 来检查 Git 的某一项配置

   <img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20201102210306.png" alt="image-20201102210305971" style="zoom:80%;" align="left"/>

---

#### **git config**

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。

```CMD
usage: git config [<options>]

# 设置 config文件 
Config file location
        --global              use global config file
        --system              use system config file
    --local               use repository config file
    --worktree            use per-worktree config file
    -f, --file <file>     use given config file
    --blob <blob-id>      read config from given blob object
# 对配置表 CRUD
Action
    --get                 get value: name [value-regex]
    --get-all             get all values: key [value-regex]
    --get-regexp          get values for regexp: name-regex [value-regex]
    --get-urlmatch        get value specific for the URL: section[.var] URL
    --replace-all         replace all matching variables: name value [value_regex]
    --add                 add a new variable: name value
    --unset               remove a variable: name [value-regex]
    --unset-all           remove all matches: name [value-regex]
    --rename-section      rename section: old-name new-name
    --remove-section      remove a section: name
    -l, --list            list all
    -e, --edit            open an editor # 打开编辑器
    --get-color           find the color configured: slot [default]
    --get-colorbool       find the color setting: slot [stdout-is-tty]

Type
    -t, --type <>         value is given this type
    --bool                value is "true" or "false"
    --int                 value is decimal number
    --bool-or-int         value is --bool or --int
    --bool-or-str         value is --bool or string
    --path                value is a path (file or directory name)
    --expiry-date         value is an expiry date

Other
    -z, --null            terminate values with NUL byte
    --name-only           show variable names only
    --includes            respect include directives on lookup
    --show-origin         show origin of config (file, standard input, blob, command line)
    --show-scope          show scope of config (worktree, local, global, system, command)
    --default <value>     with --get, use default value when missing entry
```

---

#### **通用配置**

##### **用户信息**

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```console
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

##### **设置文本编译器** 

既然用户信息已经设置完毕，你可以配置默认文本编辑器了，当 Git 需要你输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑器。

如果你想使用不同的文本编辑器，例如 Emacs，可以这样做：

```console
$ git config --global core.editor emacs
```

在 Windows 系统上，如果你想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。 它可能随你的编辑器的打包方式而不同。

对于 Notepad++，一个流行的代码编辑器来说，你可能想要使用 32 位的版本， 因为在本书编写时 64 位的版本尚不支持所有的插件。 如果你在使用 32 位的 Windows 系统，或在 64 位系统上使用 64 位的编辑器，那么你需要输入如下命令：

```console
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

| Note | Vim、Emacs 和 Notepad++ 都是流行的文本编辑器，通常程序员们会在 Linux 和 macOS 这类基于 Unix 的系统或 Windows 系统上使用它们。 如果你在使用其他的或 32 版本的编辑器，请在 [`core.editor`](https://git-scm.com/book/zh/v2/ch00/_core_editor) 中查看设置为该编辑器的具体步骤。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Warning | 如果你不这样设置编辑器，那么当 Git 试图启动它时你可能会被弄糊涂、不知所措。 例如，在 Windows 上 Git 在开始编辑时可能会过早地结束。 |
| ------- | ------------------------------------------------------------ |
|         |                                                              |

---

----

### 1.2Git基本使用

#### 获取 Git 仓库

通常有两种获取 Git 项目仓库的方式：

1. 将尚未进行版本控制的本地目录转换为 Git 仓库；
2. 从其它服务器 **克隆** 一个已存在的 Git 仓库。

两种方式都会在你的本地机器上得到一个工作就绪的 Git 仓库。

##### 1.在已存在目录中初始化仓库

如果你有一个尚未进行版本控制的项目目录，想要用 Git 来控制它，那么首先需要进入该项目目录中。

之后执行：

```
$ git init
```

该命令将创建一个名为 `.git` 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。 

如果在一个已存在文件的文件夹（而非空文件夹）中进行版本控制，你应该开始追踪这些文件并进行初始提交。 可以通过 `git add` 命令来指定所需的文件来进行追踪，然后执行 `git commit` ：

```console
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

稍后我们再逐一解释这些指令的行为。 现在，你已经得到了一个存在被追踪文件与初始提交的 Git 仓库。



---

#### 记录每次更新到仓库

现在我们的机器上有了一个 **真实项目** 的 Git 仓库，并从这个仓库中检出了所有文件的 **工作副本**。 通常，你会对这些文件做些修改，每当完成了一个阶段的目标，想要将记录下它时，就将它提交到到仓库。

<u>请记住，你工作目录下的每一个文件都不外乎这两种状态：**已跟踪** 或 **未跟踪**。</u> 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件。



工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git 刚刚检出了它们， 而你尚未编辑过它们。



编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 在工作时，你可以选择性地将这些修改过的文件放入暂存区，然后提交所有已暂存的修改，如此反复。



<img src="https://git-scm.com/book/en/v2/images/lifecycle.png" alt="Git 下文件生命周期图。" style="zoom:80%;" align="left"/>

- Figure 8. 文件的状态变化周期	

##### 检查当前文件状态

可以用 `git status` 命令查看哪些文件处于什么状态。 如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

```console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。 此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。 最后，该命令还显示了当前所在分支，并告诉你这个分支同远程服务器上对应的分支没有偏离。 



现在，让我们在项目下创建一个新的 `README` 文件。 如果之前并不存在这个文件，使用 `git status` 命令，你将看到一个新的未跟踪文件：

```console
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的 `README` 文件出现在 `Untracked files` 下面。 未跟踪的文件意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”。 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。 不过现在的例子中，我们确实想要跟踪管理 `README` 这个文件。



##### 跟踪新文件

使用命令 `git add` 开始跟踪一个文件。 所以，要跟踪 `README` 文件，运行：

```console
$ git add README
```

此时再运行 `git status` 命令，会看到 `README` 文件已被跟踪，并处于暂存状态：

```console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

    new file:   README
```

只要在 `Changes to be committed` 这行下面的，就说明是已暂存状态。 如果此时提交，那么该文件在你运行 `git add` 时的版本将被留存在后续的历史记录中。 你可能会想起之前我们使用 `git init` 后就运行了 `git add <files>` 命令，开始跟踪当前目录下的文件。 `git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。



##### 暂存已修改的文件

现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 `CONTRIBUTING.md` 的已被跟踪的文件，然后运行 `git status` 命令，会看到下面内容：

```console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

文件 `CONTRIBUTING.md` 出现在 `Changes not staged for commit` 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。 要暂存这次更新，需要运行 `git add` 命令。 <u>这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。</u> 将这个命令理解为“精确地将内容添加到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 现在让我们运行 `git add` 将“CONTRIBUTING.md”放到暂存区，然后再看看 `git status` 的输出：

```console
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```



现在两个文件都已暂存，下次提交时就会一并记录到仓库。 <u>假设此时，你想要在 `CONTRIBUTING.md` 里再加条注释。 重新编辑存盘后，准备好提交。</u> 不过且慢，再运行 `git status` 看看：

```console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

怎么回事？ 现在 `CONTRIBUTING.md` 文件同时出现在暂存区和非暂存区。 这怎么可能呢？ 好吧，实际上 Git 只不过暂存了你运行 `git add` 命令时的版本。 如果你现在提交，`CONTRIBUTING.md` 的版本是你最后一次运行 `git add` 命令时的那个版本，而不是你运行 `git commit` 时，在工作目录中的当前版本。 所以，运行了 `git add` 之后又作了修订的文件，需要重新运行 `git add` 把最新版本重新暂存起来：

```console
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```

##### 状态简览

`git status` 命令的输出十分详细，但其用语有些繁琐。 Git 有一个选项可以帮你缩短状态命令的输出，这样可以以简洁的方式查看更改。 如果你使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种格式更为紧凑的输出。

```console
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。 

输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。例如，上面的状态报告显示： `README` 文件在工作区已修改但尚未暂存，而 `lib/simplegit.rb` 文件已修改且已暂存。 `Rakefile` 文件已修，暂存后又作了修改，因此该文件的修改中既有已暂存的部分，又有未暂存的部分。

> 有待理解。



##### 忽略文件

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件的模式。



来看一个实际的 `.gitignore` 例子：

```console
$ cat .gitignore
*.[oa]
*~
```

第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二行告诉 Git 忽略所有名字以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。 此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。 要养成一开始就为你的新仓库设置好 .gitignore 文件的习惯，以免将来误提交这类无用的文件。

文件 `.gitignore` 的格式规范如下：

- 所有空行或者以 `#` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（`!`）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（`*`）匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）。 使用两个星号（`**`）表示匹配任意中间目录，比如 `a/**/z` 可以匹配 `a/z` 、 `a/b/z` 或 `a/b/c/z` 等。

```.gitignore
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件 拒绝最大
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

> **Tip**: 
>
> GitHub 有一个十分详细的针对数十种项目及语言的 `.gitignore` 文件列表， 你可以在 https://github.com/github/gitignore 找到它。
>
> **Note**:
>
> 在最简单的情况下，一个仓库可能只根目录下有一个 `.gitignore` 文件，它递归地应用到整个仓库中。 然而，子目录下也可以有额外的 `.gitignore` 文件。子目录中的 `.gitignore` 文件中的规则只作用于它所在的目录中。 （Linux 内核的源码库拥有 206 个 `.gitignore` 文件。）
>
> 多个 `.gitignore` 文件的具体细节超出了本书的范围，更多详情见 `man gitignore` 。

---

##### 查看已暂存和未暂存的修改

如果 `git status` 命令的输出对于你来说过于简略，而你想知道具体修改了什么地方，可以用 `git diff` 命令。 稍后我们会详细介绍 `git diff`。<u>你通常可能会用它来回答这两个问题：当前做的哪些更新尚未暂存？ 有哪些更新已暂存并准备好下次提交？</u> 虽然 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，但 `git diff` 能通过文件补丁的格式更加具体地显示哪些行发生了改变。

​	

> 暂且不看。

、

##### 提交更新

现在的暂存区已经准备就绪，可以提交了。 在此之前，请务必确认还有什么已修改或新建的文件还没有 `git add` 过， 否则提交的时候不会记录这些尚未暂存的变化。 这些已修改但未暂存的文件只会保留在本地磁盘。 所以，每次准备提交前，先用 `git status` 看下，你所需要的文件是不是都已暂存起来了， 然后再运行提交命令 `git commit`：

```console
$ git commit
```

这样会启动你选择的文本编辑器来输入提交说明。

> 启动的编辑器是通过 Shell 的环境变量 `EDITOR` 指定的，一般为 vim 或 emacs。 当然也可以按照 [起步](https://git-scm.com/book/zh/v2/ch00/ch01-getting-started) 介绍的方式， 使用 `git config --global core.editor` 命令设置你喜欢的编辑器。

退出编辑器时，Git 会丢弃注释行，用你输入的提交说明生成一次提交。

另外，你也可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行，如下所示：

```console
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

请记住，提交时记录的是放在暂存区域的快照。 任何还未暂存文件的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。



##### 跳过使用暂存区域

<font color="blue">尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 </font>Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就	会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤：

##### 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 “Changes not staged for commit” 部分（也就是 *未暂存清单*）看到：



下一次提交时，该文件就不再纳入版本管理了。 <u>如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `-f`（译注：即 force 的首字母）。</u> <u>这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。</u>



另外一种情况是，<u>我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。</u>

(和add命令刚好相反，add既可以跟踪文件，亦可以将追踪文件加入暂存区) 。

换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 `.a` 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 选项：

```console
$ git rm --cached README
```

`it rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。比如：

```console
$ git rm log/\*.log
```

注意到星号 `*` 之前的反斜杠 `\`， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 `log/` 目录下扩展名为 `.log` 的所有文件。 类似的比如：

```console
$ git rm \*~
```

该命令会删除所有名字以 `~` 结尾的文件。



##### 移动文件

不像其它的 VCS 系统，Git 并不显式跟踪文件移动操作。 如果在 Git 中重命名了某个文件，仓库中存储的元数据并不会体现出这是一次改名操作。 不过 Git 非常聪明，它会推断出究竟发生了什么，至于具体是如何做到的，我们稍后再谈。

既然如此，当你看到 Git 的 `mv` 命令时一定会困惑不已。 要在 Git 中对文件改名，可以这么做：

```console
$ git mv file_from file_to
```

它会恰如预期般正常工作。 实际上，即便此时查看状态信息，也会明白无误地看到关于重命名操作的说明：

```console
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

其实，运行 `git mv` 就相当于运行了下面三条命令：

```console
$ mv README.md README
$ git rm README.md
$ git add README
```

如此分开操作，Git 也会意识到这是一次重命名，所以不管何种方式结果都一样。 两者唯一的区别是，`mv` 是一条命令而非三条命令，直接用 `git mv` 方便得多。 不过有时候用其他工具批处理重命名的话，要记得在提交前删除旧的文件名，再添加新的文件名。

---

---

### 1.3Git内部原理

 首先要弄明白一点，从根本上来讲 Git 是一个**内容寻址（content-addressable）文件系统**，并在此之上提供了一个版本控制系统的用户界面。 马上你就会学到这意味着什么。

 	

#### **底层命令与上层命令**

本书主要涵盖了 `checkout`、`branch`、`remote` 等约 30 个 Git 的子命令。 然而，由于 Git 最初是一套面向版本控制系统的工具集，而不是一个完整的、用户友好的版本控制系统， 所以它还包含了一部分用于完成底层工作的子命令。 这些命令被设计成能以 UNIX 命令行的风格连接在一起，抑或藉由脚本调用，来完成工作。 这部分命令一般被称作“底层（plumbing）”命令，而那些更友好的命令则被称作“上层（porcelain）”命令。



你或许已经注意到了，本书前九章专注于探讨上层命令。 然而在本章中，我们将主要面对底层命令。 因为，底层命令得以让你窥探 Git 内部的工作机制，也有助于说明 Git 是如何完成工作的，以及它为何如此运作。 多数底层命令并不面向最终用户：它们更适合作为新工具的组件和自定义脚本的组成部分。



当在一个新目录或已有目录执行 `git init` 时，Git 会创建一个 `.git` 目录。 这个目录包含了几乎所有 Git 存储和操作的东西。 如若想备份或复制一个版本库，只需把这个目录拷贝至另一处即可。 本章探讨的所有内容，均位于这个目录内。 新初始化的 `.git` 目录的典型结构如下：

```console
$ ls -F1
config
description
HEAD
hooks/
info/
objects/
refs/
```

随着 Git 版本的不同，该目录下可能还会包含其他内容。 不过对于一个全新的 `git init` 版本库，这将是你看到的默认结构。 `description` 文件仅供 GitWeb 程序使用，我们无需关心。 `config` 文件包含项目特有的配置选项。 `info` 目录包含一个全局性排除（global exclude）文件， 用以放置那些不希望被记录在 `.gitignore` 文件中的忽略模式（ignored patterns）。 `hooks` 目录包含客户端或服务端的hook scripts， 在 [Git hook](https://git-scm.com/book/zh/v2/ch00/_git_hooks) 中这部分话题已被详细探讨过。



<u>剩下的四个条目很重要：`HEAD` 文件、（尚待创建的）`index` 文件，和 `objects` 目录、`refs` 目录。</u> 

<u>它们都是 Git 的核心组成部分。 `objects` 目录存储所有数据内容；`refs` 目录存储指向数据（分支、远程仓库和标签等）的提交对象的指针； `HEAD` 文件指向目前被检出的分支；`index` 文件保存暂存区信息。</u> 我们将详细地逐一检视这四部分，来理解 Git 是如何运转的。





---

---



### 2. Github

#### 1. git连接github

[参见blog](https://www.cnblogs.com/ayseeing/p/3572582.html)



#### 2.  远程仓库

##### 1. 创建本地仓库

1. 将尚未进行版本控制的本地目录转换为 Git 仓库； `git init`
2. 从其它服务器 **克隆** 一个已存在的 Git 仓库。            `git clone <url>`

##### 2. 配置远程仓库

1. 查看远程仓库 `git remote` 显示服务器简写,`git remote -v` 显示 url

   ` git remote show <shortname>` 显示仓库的具体信息

2. 添加远程仓库  `git remote add <shortname> <url>`   

<font color="red">可以add 仓库不代表有权限 ，必须可以 `git push origin master` </font>

3. 修改远程仓库的简写名

   `git remote rename <origin_name> <new_name>`

4. 移除仓库 

   `git remote remove <shortname>`

---

### 3. IDEA集成Git

#### 0.git连接github

#### 1. 项目共享到GitHub

VCS->Import into Version Control->Share Project on GitHub

共享到GitHub，打开自己的GitHub可看到自动创建了一个项目

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE2MzkyNjY1Mz93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

#### 2.项目Push到GitHub

右击项目，先把项目提交到本地，即添加Git->Add，提交Git->Commit Directory。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE2NDQ0NDIzMD93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

提交(Commit)的时候，出现如下界面。选择Commint，提交到本地Git；选择Commit and Push，提交到本地并Push到远程库(GitHub)。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE2NDk1MzIzNz93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

如果选择Commit提交本地后，则可以用Git->Repository->Push推送到GitHub

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE2NTQ1MDQ2Mz93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

#### 3.查看历史提交

在由上角有Show History的按钮，可以查看历史提交

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE3MDAxNTQwOD93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

Head当前版本指向，master本地版本指向，origin/master远程版本指向

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxMzE3MDEwNzk5Mj93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDB4MVkydGZXbG89L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

[将idea项目提交到Github](https://blog.csdn.net/Luck_ZZ/article/details/79541093)

https://juejin.im/post/6844903926576185358

### 版本管理

#### **1. 合并文件**

---

**三向合并**

在看怎么合并两个分支之前，我们先来看一下怎么合并两个文件，因为两个文件的合并是两个分支合并的基础。

大家应该都听说过<font color="blue">三向合并</font>这个词，不知道大家有没有思考过为什么两个文件的合并需要三向合并，只有二向是否可以自动完成合并？？？？

如下图

<img src="https://pic3.zhimg.com/v2-8308f536b1986fd877fd360cbd6e9ed9_b.jpg" alt="img"   align="left"/>

很明显答案是不能。如上图的例子，Git 没法确定这一行代码是我修改的，还是对方修改的，或者之前就没有这行代码，是我们俩同时新增的。此时 Git 没办法帮我们做自动合并。

所以我们需要三向合并，所谓三向合并，就是找到两个文件的一个**合并 base**。

如下图，这样子 Git 就可以很清楚的知道说，对方修改了这一行代码，而我们没有修改，自动帮我们合并这两个文件为 Print("hello")。

<img src="https://pic1.zhimg.com/80/v2-c8ad9474d401b2f1128980911ad3d9b0_720w.jpg" alt="img" style="zoom:80%;" align="left"/>



---

**文件冲突**

接下来我们了解一下什么是冲突？冲突简单的来说就是三向合并中的三方都互不相同，即参考合并 base，我们的分支和别人的分支都对同个地方做了修改。

<img src="https://pic2.zhimg.com/v2-763962194d688dad1a479d505f1d8485_r.jpg" alt="preview" style="zoom:80%;" align="left"/>

---

#### **2.合并策略** 

![img](https://pic2.zhimg.com/80/v2-5e0819fd060ecf654a7fa33309444ae2_720w.jpg)

了解完怎么合并两个文件之后，我们来看一个使用 git merge 来做分支合并。如上图，将 master 分支合并到 feature 分支上，会新增一个 commit 节点来记录这次合并。

Git 会有很多合并策略，其中常见的是 Fast-forward、Recursive 、Ours、Theirs、Octopus。下面分别介绍不同合并策略的原理以及应用场景。默认 Git 会帮你自动挑选合适的合并策略，如果你需要强制指定，使用`git merge -s <策略名字>`

了解 Git 合并策略的原理可以让你对 Git 的合并结果有一个准确的预期。

---

##### **Fast-forward**

![img](https://pic2.zhimg.com/v2-360bc75abbe0fdd5bbbd0dc6950caaf5_b.jpg)

Fast-forward 是最简单的一种合并策略，如上图中将 some feature 分支合并进 master 分支，Git 只需要将 master 分支的指向移动到最后一个 commit 节点上。

![img](https://pic4.zhimg.com/80/v2-e881bee3a250dd0aca96b6a11241ab78_720w.jpg)

Fast-forward 是 Git 在合并两个没有分叉的分支时的默认行为，如果不想要这种表现，想明确记录下每次的合并，可以使用`git merge --no-ff`。











---

### git换行符

- 关于crlf 和 lf 的恩恩怨怨

##### 1. 探究起因：使用Clion出现error:

The following problems have occurred when adding the files:
LF would be replaced by CRLF in

[我查了一篇博客](https://blog.csdn.net/Michaeles/article/details/84615560)

做法 <font color="blue">设置config的autocrlf = false</font> 解决问题

但是我不懂<font color="red">autocrlf ， safecrlf到底做了什么 为什么需要他们</font>？？

于是有了以下部分。

##### CRLF与LF的历史遗留问题

- 起因：<font color="blue">不同系统换行符不同 </font>

  而git捕获到了这个·异常

这是因为Windows使用回车(carriage return )和换行(line feed)两个字符来结束一行，而Mac和Linux只使用换行一个字符。虽然这是小问题，但它会极大地扰乱跨平台协作。

Windows自带的文本编辑器默认CRLF，linux则是LF.

##### 2.补救措施

###### 1.用户的补救措施

<font color="red">可是用户可以将默认的CRLF换行改成LF,。</font><font color="blue">这是一个坑，下面会提及。</font>

###### 2.git的补救措施

1. core.safecrlf--Git的换行符检查功能

- 作用：git add时检查暂存区文件是否混用了不同风格的换行符
- false - 不做任何检查
  warn - 在提交时检查并警告
  true - 在提交时检查，如果发现混用则拒绝提交

2. Git的自动转换功能。core.autocrlf 

- 作用:当添加到暂存区时，自动将CRLF转换成LF；反之，当检出时，自动将LF转换成CRLF。

- Git可以在你提交(add)时自动地把行结束符CRLF转换成LF而在签出(clone)代码时把LF转换成CRLF。
  3. git提交CRLF文件到github会发出警告，并拒绝。

<font color="red">本来这很完美，windows系统用户只要把autocrlf,safecrlf都设为true,windows</font>

##### 3.同时使用以上两种方法后果

有的人上面两种都使用了(比如我:cry:),恭喜你--成功报错。

你傻呀? Github相当于liunx ， 你是 windows。

<font color="red">提交到github应该是LF换行，</font>

<font color="red">需要转换是因为WIndows文本编辑器以及文本默认CRLF格式不</font>

<font color="red">符合，但是一旦把IDE的换行符改成一致的LF那就不需要转换。</font>

<font color="red">此时Github与我相当于同一系统，可是我autocrlf = True 我两又</font>

<font color="red">变成不同系统了。</font>

就好像相反数的转换，你做了两次不就啥也没干吗

##### 4.推荐做法：

autocrlf = false,你的文本编辑器(Nodepad++)默认用LF.(行业规范)。

safecrlf = true, 这个最好开。原因不解释(我解释不清楚，就不瞎扯了)

##### 补充一下git的配置问题

###### 两种配置方法

- 命令行，键入`git config --global core.safecrlf true`

- 找到对应文件目录下 .git文件夹(这是隐藏的) 打开config文件输入对应信息。

###### 两种配置方式

- git config --global core.safecrlf true
- git config core.safecrlf true

###### 怎么理解呢？

1. 命令行与文件配置的关系

首先 git init 会在对应的目录下生成.git隐藏文件 。

当你按照网上git入门教程  配置信息 。

点开相应的config文件(一开始是空的) 你输入啥，文件里就多了啥 。

包括什么 user,email,远端仓库，以及我们提到的autocrlf ，safecrlf .

<font color="red">结论：等价关系。</font>

2. global咋回事？

很简单，以C语言为例，你有一个全局变量 A=True ; 

在这个文件里你在哪里都可以使用它，它的值是True.

即使某个区域没有你一可以用它。

<font color="red">我说这话什么意思？</font>

命令行里输入`git config --global core.safecrlf true`，但是新建项目里面的config根本没有 safecrlf这个参数。就像C里面，<font color="red">如果没有就去上一层找. </font>global是你下载的Git 文件夹里的配置。同样，如果你的局部有一个A = False,

学过编程的人都知道，这会覆盖。在这个项目里A的值就是False.

<font color="blue">简单说就是全局变量和本地变量那回事</font>,

global是全局参量，即<font color="red">默认设置</font>.如果局部config没有相应的就沿用全局变量

---



##### 验证：

基于以上的理解用Clion+git做了相关测试，有兴趣看看也没啥意义。

###### autocrlf报错情况

- autocrlf=false clion文件CRLF换行 

  它警告：你现在将提交CRLF文件到github，建议autocrlf = True 避免换行符issue

- Clion文件换行符 LF 可以提交

- autocrlf = True  Clion换行符LF

  The following problems have occurred when adding the files:
  LF would be replaced by CRLF in 数据结构/test.c

结论 IDE 使用 LF换行 autcrlf = false 



###### safecrlf报错情况

- test文件CRLF test文件LF

  The following problems have occurred when adding the files:
  LF would be replaced by CRLF in 数据结构/tset1.c

- safecrlf = false 可以add

  所以那些说safecrlf = false来解决问题的人压根不懂.

  <font color="red">这种做法就是自欺欺人.解决bug的方法竟然是让编译器忽略它</font>





---

---

> 学习视频:
>
> 链接：https://pan.baidu.com/s/1PW4sBXX_5u6BgfMIiVzBSA 
> 提取码：bbpm 
> 复制这段内容后打开百度网盘手机App，操作更方便哦--来自百度网盘超级会员V3的分享
>
> 参考:
>
> [GitBook](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

---















#### 回退版本

预备知识：

> 一个commit对应这一个版本，有一个commit id，40位的16进制数字，通过SHA1计算得到，不同的文件计算出来的SHA1值不同(有很小的几率相同，可忽略)，这样每一个提交都有其独特的id。每提交一个新版本，实际上Git就会把它们自动串成一条时间线。
> 在Git中，HEAD表示当前版本，也就是e620a6ff0940a8dff…，HEAD^表示上一个版本，HEAD^^表示上上一个版本，往上100个版本可以写成HEAD加连续100个^，也可以写成：HEAD~100。
>
> - 现在^^不能用了



**git log：**该命令显示从最近到最远的提交日志。

```
commit e620a6ff0940a8dff91e0d252f30e4d138ec37be
Author: TangShengqin <15527733782@163.com>
Date:   Wed Jan 3 10:35:44 2018 +0800
    练习版本回退，假设这是版本3
commit 33342d9870f104719d351539a15e74a1382407ea
Author: TangShengqin <15527733782@163.com>
Date:   Wed Jan 3 10:34:03 2018 +0800
    练习版本回退，假设这是版本2
......
```

**git log - -pretty=oneline：**将只会显示提交的commit id号和对应的注释。(这里是两个-，Markdown显示两个-为一个-)

```
e620a6ff0940a8dff91e0d252f30e4d138ec37be 练习版本回退，假设这是版本3
33342d9870f104719d351539a15e74a1382407ea 练习版本回退，假设这是版本2
......
```

**git reset –hard id** 之前的版本日志信息：

```
git log --pretty=oneline
e620a6ff0940a8dff91e0d252f30e4d138ec37be 练习版本回退，假设这是版本3
33342d9870f104719d351539a15e74a1382407ea 练习版本回退，假设这是版本2
```

**git reset –hard commit_id 或则是 git reset –hard HEAD~1**  || *^不起作用了*

```
git reset --hard HEAD~1   # hard选项，表示彻底将工作区、暂存区和版本库记录恢复到指定的版本库
```

使用git reset –hard 进行版本回退之后，在本地查看README.md，里面已经变为版本2对应的内容了。



**如果你在本地做了错误提交，那么回退版本的方法很简单**
1.先用下面命令找到要回退的版本的commit id：

```
git reflog 
```

2.接着回退版本:

```
git reset --hard a7e1d2791
```

a7e1d279就是你要回退的版本的commit id的前面几位。



---





---





---

## Git 常用命令



### branch 

```
git branch -- 查看分支 
git branch -a -- 查看所有分支（包括remote）
git checkout -b dev -- 创建分支并切换

git pull/push origin dev:dev -- 指定分支pull,push 前面是云端 后面是本地

git branch rm <branchName> -- 删除分支 
git push origin --delete <branchName> -- 删除r

```



### commit : amend

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令来重新提交：

```console
$ git commit --amend
```

这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令）， 那么快照会保持不变，而你所修改的只是提交信息。

---

### Log ：查看提交记录

```
1.git log
如果不带任何参数，它会列出所有历史记录，最近的排在最上方，显示提交对象的哈希值，作者、提交日期、和提交说明。如果记录过多，则按Page Up、Page Down、↓、↑来控制显示；按q退出历史记录列表。

2.git log -n
如果不想向上面那样全部显示，可以选择显示前N条。

3.git log --stat -n
显示简要的增改行数统计,每次提交文件的变更统计，-n 同上，前n条，可省略。

4.git log -p -n
此命令同上，不过显示更全了。

5.git log --pretty=oneline
一行显示，只显示哈希值和提交说明。

6.git log --help
Launching default browser to display HTML ...

```

---

### rm ： 删除

```
当需要删除暂存区或分支上的文件，同时工作区不需要这个文件
git rm fileName 

当需要删除暂存区或分支上的文件，同时工作区需要这个文件，但是不需要被版本控制
后添加进.gitignore文件中的文件可以使用这条命令解除版本控制的追踪，然后在commit忽略这个文件。
git rm --cache fileName

```

简单说 --cache只是取消track



git rm --cached .idea/\*.* 取消对目录所有文件的追踪 

---

### remote

```
假如远程分支地址：https://xxxxxxx/wangdong/helloworld.git 
将本地关联到远程  
1.git remote add origin https://xxxxxxx/wangdong/helloworld.git 
2.git push -u origin master 
取消关联远程分支  
git remote remove origin 
查看与远端分支关联的状态
git remote -v
查看本地分支与远端分支的关联状态
git branch -v
```



### tag



#### 后期打标签

你也可以对过去的提交打标签。 假设提交历史是这样的：

```console
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
9fceb02d0ae598e95dc970b74767f19372d61af8 updated 
```

现在，假设在 v1.2 时你忘记给项目打标签，也就是在 “updated rakefile” 提交。 你可以在之后补上标签。 要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）：

```console
$ git tag -a v1.2 9fceb02
```

---

#### 共享标签

默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样——你可以运行 `git push origin <tagname>`。

#### **delete tags**

在idea中由于没有找到删除标签的功能，所以只能采用命令行的方式进行

进入工程目录-->右键，Git Bash Here进入命令窗口

<img src="https://img2018.cnblogs.com/blog/560071/201809/560071-20180919012402329-861123150.png" alt="img" style="zoom:80%;" align="left"/>

 

删除本地标签命令
`git tag -d v1.0.1`
删除远程标签命令
`git push origin :refs/tags/v1.0.1`



---

### restore 

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

在S3的状态下，使用restore恢复到s2(不可以在编辑器中手动将修改为S2的内容)，后

重新增加一行错误数据 ，并更新到暂存区。状态标记为S4

![image-20210712152749170](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712152749.png)

```
git restore --staged a.txt
fatal: could not resolve HEAD
```

我猜想restore命令本质是恢复(重写)而不是删除。

这个命令会从版本库中 查找 a.txt的最新版本然后覆盖暂存区原来的文件。

将这个错误标记为E1 

> restore 用于恢复对工作区的修改(恢复为暂存区，或者版本库的内容)
>
> 此时工作区与暂存区内容相同，因此restore操作是无效的

---

验证E1

 在S3下,更新暂存区，并提交。状态为标记S5

![image-20210712155204423](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210712155204.png)

在工作区添加一行错误信息，并且通过add同步到暂存区，(暂存区与版本库不一致)

此时使用 `restore --staged`会将暂存区的add的内容丢弃，使其与版本库一直。





`no changes added to commit`说明之前的add操作撤销了，`modified： a.txt`说明工作区的修改没有被撤销

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

> 图中 暂存区内容与版本库一致

---

与rm --cache 不同 restore只是撤销修改的文件，但是并未取消对文件的追踪。

- 通过参数指定撤销工作区还是暂存区的修改(一般都是暂存区)

---

E1错误的发生在文件未被commit原因不知。



# Git 概念

## Git 基本流程

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。

本章将对有关创建与提交你的项目快照的命令作介绍。

Git 常用的是以下 6 个命令：**git clone**、**git push**、**git add** 、**git commit**、**git checkout**、**git pull**，后面我们会详细介绍。

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

**说明：**

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库



## Git 工作区、暂存区和版本库

------

### 基本概念

我们先来理解下 Git 工作区、暂存区和版本库概念：

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![img](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

- 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
- 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。(<font color="yellow">相当于commit的反向操作，使暂存区版本与master分支一致</font>)
- 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
- 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

```
git diff 比较工作区与暂存区的差异

git reset 回退版本(是指工作区回到之前的某个版本)


```



---

