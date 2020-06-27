# Git 总结

## 1. Git 简介

- Git 是目前最先进的分布式版本控制系统

- 其他分布式版本控制系统：CVS，SVN，ClearCase，VSS，Mercurial，Bazaar等

- 集中式 v.s. 分布式

  集中式：数据库集中存放在中央服务器。当开始工作时，需要从中央服务器获取最新版本；结束工作后，再将内容推送给中央服务器。![central-repo](https://www.liaoxuefeng.com/files/attachments/918921540355872/l)

  其最大的缺点是必须联网才能工作，数据在互联网中传输速度较慢。

  集中式：每个人拥有一个完整的版本库，但通常分布式版本控制系统有一台充当“中央服务器”的电脑，用于方便大家交换数据。![distributed-repo](https://www.liaoxuefeng.com/files/attachments/918921562236160/l)

  分布式系统安全性更高，当某个人电脑出现问题时，只需从他处复制一份即可。而集中式系统一旦中央服务器出现问题，所有人都无法继续工作。

## 2. Windows 下 Git 的安装与配置

```bash
$ git config --global user.name "Jeff1999"           # 用户名
$ git config --global user.email "yifan-luo@qq.com"  # 邮箱
```

`git config` 命令的 `--global` 参数表示这台机器上所有的 Git 仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和邮箱账户。

## 3. 创建并初始化 Git 版本库

版本库又名仓库（Repository），可以被简单理解成一个目录。其中所有文件都可以被 Git 管理起来，每个文件的修改和删除都可以被追踪，以便在任何时刻查看历史，或者还原版本。

```bash
$ mkdir learngit          # 创建空目录
$ cd learngit             # 进入空目录
$ pwd                     # 显示当前路径
/Users/michael/learngit
$ git init                # 将空目录初始化为 Git 空仓库
Initialized empty Git repository in /Users/michael/learngit/.git/
```

新生成的 `.git` 隐藏目录用来跟踪管理版本库，禁止随意手动修改其下文件。

除了在空目录下创建 Git 仓库，也可以在已经生成的目录下创建 Git 仓库。

## 4. 添加并提交文件

所有的版本控制系统，只能追踪文本文件（txt文件、网页、代码等）的改动；而图片和视频等二进制文件虽然可以进行版本控制系统管理，但无法追踪文件变化。

建议统一使用 `UTF-8` 编码编写文本文件。

首先创建 `readme.txt` 文件，内容如下：

```txt
Git is a version control system.
Git is free software.
```

添加文件到仓库：

```bash
$ git add readme.txt      # 添加文件至仓库
```

把文件提交到仓库：

```bash
$ git commit -m 'wrote a readme file'      # 提交文件至仓库
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`git commit` 命令执行成功后会显示：`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

还可以一次添加多个文件，最后再提交到仓库：

```bash
$ git add file1.txt                
$ git add file2.txt file3.txt      # 添加多个文件 
$ git commit -m "add 3 files."
```

## 5. 查看当前状态和修改内容

继续修改 `readme.txt` 如下：

```txt
Git is a distributed version control system.
Git is free software.
```

运行 `git status` 查看结果：

```bash
$ git status         # 查看当前添加、提交至仓库的情况
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

结果显示，`readme.txt` 已被修改 ，但还没有被添加（not staged）到仓库。

运行 `git diff` 查看具体修改内容：

```bash
$ git diff readme.txt     # 查看某文件在仓库和工作区的不同
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

结果显示，我们在第一行添加了一个 `distributed` 单词。

通过查看修改后再添加、提交文件：

```bash
$ git add readme.txt
```

再次查看当前状态：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

结果显示，`readm.txt` 已被添加（`git add`）至仓库，但还尚未提交（to be committed）。

提交 `readme.txt` 到仓库：

```bash
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

最后再次查看当前仓库的状态：

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

结果显示，没有待添加和提交的文件（nothing to commit），工作目录是干净的（clean）。

## 6. 版本回退

再次修改文件如下:

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

提交文件：

```bash
$ git add readme.txt
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git 为每一次提交保存了一个“快照”，在 Git 中被称为 commit。当有文件错改或误删时，可以选择从最近的一个 commit 中恢复，然后继续工作。

版本回顾：

- 第一次提交："wrote a readme file"

```txt
Git is a version control system.
Git is free software.
```

- 第二次提交："add distributed"

```txt
Git is a distributed version control system.
Git is free software.
```

- 第三次提交："append GPL"

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

通过 `git log` 命令可以查看存有历史版本的日志：

```bash
$ git log       # 查看记载之前版本的日志
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)         # HEAD 指向当前版本
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800          # 第三次

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366 
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800          # 第二次

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0  
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800          # 第一次

    wrote a readme file
```

`commit` 后显示的十六进制序列为版本号（commit id），通过 SHA1 算法自动生成。

可以通过 `git log --pretty=online` 简化输出信息：

```bash
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

在 Git 中，通过 `HEAD` 表示当前版本，即 `1094adb...`；

`HEAD^` 表示上一个版本，即 `e475afc...`；

`HEAD^^` 表示上上一个版本，即 `eaadf4e...`；

`HEAD~100` 表示往上 100 个版本。

现在，通过 `git reset` 命令回退到上一个版本（"add distributed"）：

```bash
$ git reset --hard HEAD^
```

查看回退后的 `readme.txt` 内容：

```bash
$ cat readme.txt      # 第二次提交的版本："add distributed"
Git is a distributed version control system.
Git is free software.
```

通过 `get log` 查看版本库中的历史版本：

```bash
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)             # HEAD 指向当前版本
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800          # 第二次

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800          # 第一次

    wrote a readme file
```

返回第三次更新后的版本（"wrote a readme file"）：

```bash
$ git reset --hard 1094a                # Git 自动补全版本号
HEAD is now at 83b0afe append GPL
```

再次查看 `readme.txt` 的内容：

```txt
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

结果显示，又回到了第三次更新的版本。

Git 版本回退通过移动 `HEAD` 指针完成：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改变 `HEAD` 指针，指向上一个版本：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

如果找不到历史版本的 `commit id`，Git 提供了 `git reflog` 命令来查看之前的每一次版本改动：

```bash
$ git reflog         # 记录了每一次 HEAD 指针的变化情况
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

## 7. 工作区与缓存区

- 工作区（Working Directory）

  即电脑中可以看到的目录，例如`readme.txt` 文件夹就是一个工作区：

  ![working-dir](https://www.liaoxuefeng.com/files/attachments/919021113952544/0)

- 版本库（Repository）

  工作区中的 `.git` 隐藏目录不属于工作区，而是 Git 的版本库。

  版本库由两部分构成：暂存区（stage）和 Git 创建的第一个分支（master）：

  ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

  `git add` 用于添加文件，把文件从工作区添加至暂存区；

  `git commit` 用于提交文件，把文件从暂存区添加至当前分支。

  因为在创建 Git 版本库时，Git 自动创建了唯一一个 `master` 分支，所以，现在， `git commit` 就是往 `master` 分支上提交更改。

  可以简单理解为，需要提交的文件修改一起放到暂存区，然后再一次性提交暂存区的所有修改。

对 `readme.txt` 文件做出修改：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

再新增一个 `LICENSE.txt` 文件，内容随意。

通过 `git status` 查看工作区和版本库的文件状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt       # 有更新但未提交文件

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE                      # 未添加文件

no changes added to commit (use "git add" and/or "git commit -a")
```

`Untracked` 表示工作区的该文件尚未被添加至暂存区。

通过 `git add` 添加上述两个文件，再查看系统状态：

```bash
$ git add readme.txt LICENSE.txt     
$ git status
On branch master
Changes to be committed: 
  (use "git reset HEAD <file>..." to unstage)
    # 两个文件都被添加至暂存区，但尚未提交
	new file:   LICENSE
	modified:   readme.txt
```

暂存区状态发生变化：

![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

再通过 `git commit` 提交暂存区的所有文件：

```bash
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

查看系统状态：

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)

当前暂存区没有任何待提交文件。