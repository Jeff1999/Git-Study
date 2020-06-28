# Git 总结

- [Git 总结](#git---)
  * [1. Git 简介](#1-git---)
  * [2. Windows 下 Git 的安装与配置](#2-windows---git-------)
  * [3. 创建并初始化 Git 版本库](#3--------git----)
  * [4. 添加并提交文件](#4--------)
  * [5. 查看当前状态和修改内容](#5------------)
  * [6. 版本回退](#6-----)
  * [7. 工作区与缓存区](#7--------)
  * [8. 管理修改](#8-----)
  * [9. 撤销修改](#9-----)
  * [10. 删除修改](#10-----)
  * [11. 远程仓库简介](#11-------)
  * [12. 添加远程仓库](#12-------)
  * [13. 从远程库克隆](#13-------)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

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

## 8. 管理修改

Git 跟踪并管理的是修改记录（例如新增或删除一行，修改或删除一个字符，创建或删除一个文件），而非文件本身。

例如，对 `readme.txt` 做一个修改：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

然后再添加：

```bash
$ git add readme.txt
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

结果显示当前有一个文件被添加至暂存区，但尚未被提交。

再修改 `readme.txt`：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```bash
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
```

再查看状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

结果显示，还有一个文件在工作区被修改，但尚未添加至暂存区。

操作过程：第一次修改 -> `git add` -> 第二次修改 -> `git commit`

`git commit` 仅把暂存区的文件提交至分支，在工作区的第二次修改没有被添加至暂存区，所以没有被提交至分支。

通过 `git diff HEAD -- readme.txt` 查看工作区和版本库当前版本的区别：

```bash
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

结果显示，第二次修改并未提交。

可以通过 `git add` 和 `git commit` 继续将第二次修改添加至暂存区并提交。也可以 `git add` 添加两次修改，最后再 `git commit` 提交至分支，相当于把两次修改合并后再提交至分支。

## 9. 撤销修改

如果在 `reaedme.txt` 文件中不小心添加了一行：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

此时，修改尚未被添加至暂存区。用 `git status` 查看状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

 结果显示，有一个文件被修改，但尚未被添加至暂存区。

可以通过 `git checkout -- <file>` 丢弃工作区的修改：

```txt
$ git checkout -- readme.txt
```

`git checkout -- <file>` 命令其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除。

用 `git checkout -- <file>` 撤销修改有两种情况：

- 文件在工作区被修改，但尚未添加至暂存区。

  此时，撤销修改就回到当前版本库的状态。

- 文件在被添加至暂存区后，在工作区又被修改。

  此时，撤销修改就回到暂存区的状态。

总之，让文件回到最近一次 `git commit` 或 `git add` 时的状态。

再次查看 `readme.txt` 内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

结果显示，原先工作区的修改已经被撤回。

如果在 `readme.txt` 中修改了文件，并添加到了暂存区：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.

$ git add readme.txt
```

通过 `git status` 查看状态：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

此时，修改尚未被提交至分支。

通过 `git reset HEAD <file>` 可以把暂存区的修改撤销（unstage），重新放回工作区：

```bash
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

`git reset` 命令既可以回退版本，也可以把暂存区的修改回退到工作区。当用 `HEAD` 时，表示最新的版本。

再次查看状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

结果显示，工作区有修改，但尚未被添加至暂存区（not staged）。

丢弃工作区修改：

```bash
$ git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```

结果显示，当前没有被修改但尚未被添加至暂存区的文件（clean）。

如果将误修改的文件添加并提交至分支，通过 `git reset HEAD^` 版本回退命令可以回退到原来的版本。前提条件是，自己的本地库还没有被推送到远程库。

## 10. 删除修改

在 Git 中，删除也是一个操作。先在工作区创建一个 `test.txt` 文件，并添加提交至版本库：

```bash
$ cat test.test
This is a test.

$ git add test.txt

$ git commit -m "add test.txt"
[master b84166e] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

如果在工作区删除该文件：

```bash
$ rm test.txt
```

此时，Git 知道用户改动了文件，工作区和版本库不一致。通过 `git status`  查看当前状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

结果显示，工作区的文件已被删除，但该改动还未添加到暂存区。

可以通过 `git rm` 删除版本库中的文件， 并提交至版本库：

```bash
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

另一种情况是文件被误删，通过 `git checkout` 命令从版本库中恢复文件：

```bash
$ git checkout -- test.txt
```

`git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

## 11. 远程仓库简介

Git 是分布式版本控制系统。同一个 Git 仓库，可以分布到不同的机器上。最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

在实际应用中，GitHub 网站充当一台 24 小时开机的服务器，每个人都从这个服务器仓库克隆一份到自己的电脑上，并且各自把自己的提交推送到服务器仓库里，也可以从服务器仓库中获取别人的提交。

通过注册 GitHub账户，每个人拥有了一个免费的 Git 远程仓库。本地 Git 仓库与 GitHub 仓库之间的传输通过 SSH 加密，所以需要一下设置：

- 第一步：创建 SSH Key

  在目录 `C:\Users\DELL\.ssh` 下查找 `is_rsa` 和 `is_rsa.pub` 文件。`id_rsa` 是私钥，`is_rsa.pub` 是公钥，两个文件共同构成密钥对。

  如果没有相应的 `.ssh` 目录，打开 Git Bash，创建 SSH Key：

  ```bash
  $ ssh-keygen -t rsa -C "yifan-luo@qq.com"
  ```

  无需设置密码，一直回车，完成 SSH Key 的生成。

- 第二步：添加 SSH Key

  按照下图进行操作：

  ![github-addkey-1](https://www.liaoxuefeng.com/files/attachments/919021379029408/0)

  点击“Add Key”：

  ![github-addkey-2](https://www.liaoxuefeng.com/files/attachments/919021395420160/0)

GitHub 通过识别 SSH Key 来判断提交确实是由本人推送的，而不是别人冒充的。GitHub 知道了用户的公钥，通过与私钥配对实现身份识别。GitHub 允许用户添加多个 Key，只需把每台电脑的 Key 都添加到 GitHub 即可。

在 GitHub 上免费托管的仓库可以被任何人看到，尽量不要把敏感信息推送至 Git 远程仓库。用户也可以创建自己的 Git 服务器。

## 12. 添加远程仓库

首先，在 GitHub 创建一个远程仓库，命名为 “learngit”：

![github-create-repo-1](https://www.liaoxuefeng.com/files/attachments/919021631860000/0)

![github-create-repo-2](https://www.liaoxuefeng.com/files/attachments/919021652277920/0)

GitHub 提示我们：

- 可以克隆出新的仓库
- 可以关联到一个已有的本地仓库，再把本地仓库推送到远程仓库

根据 GitHub 提示，在本地的 `learngit` 仓库下运行如下命令：

```bash
$ git remote add origin git@github.com:Jeff1999/learngit.git
```

添加后，远程库的默认名称为 `origin`。

下一步，将本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
Enumerating objects: 23, done.
Counting objects: 100% (23/23), done.
Delta compression using up to 8 threads
Compressing objects: 100% (18/18), done.
Writing objects: 100% (23/23), 1.88 KiB | 385.00 KiB/s, done.
Total 23 (delta 6), reused 0 (delta 0)
remote: Resolving deltas: 100% (6/6), done.
To github.com:Jeff1999/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

`git push` 命令将当前分支 `master` 推送到远程。

由于远程库是空的，当第一次推送 `master` 分支时要加上参数 `-u`，使得 Git 不但会把本地库的 `master` 分支内容推送到远程库的 `master` 分支，还会将本地的 `master` 分支与远程的 `master` 分支关联起来，在以后的推送或拉取时可以简化命令。

推送成功后，可以在 GitHub 上看到远程库的内容和本地库已经一模一样：

![github-repo](https://www.liaoxuefeng.com/files/attachments/919021675995552/0)

今后，只要在本地做了提交，就可以通过以下命令将本地的 `master` 分支的最新版本推送至 GitHub：

```bash
$ git push origin master
```

## 13. 从远程库克隆

首先，登陆 GitHub 并创建一个新的仓库 `gitskills`：

![github-init-repo](https://www.liaoxuefeng.com/files/attachments/919021808263616/0)

![github-init-repo-2](https://www.liaoxuefeng.com/files/attachments/919021836828288/0)

下一步，通过命令 `git clone` 从远程库克隆一个本地库：

```bash
$ git clone git@github.com:Jeff1999/gitskills.git
Cloning into 'gitskills'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

此时工作区生成了一个 `gitskills` 目录，目录中包含了一个 `README.md` 文件：

```bash
$ cd gitskills
$ ls
README.md
```

