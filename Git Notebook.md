# Git - Notebook



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



## 12. 添加并推送至远程仓库

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



## 14. 分支管理简介 

Git 的分支管理使得用户可以创建自己的分支，并且他人无法看到。用户在自己的分支上工作，可以随意提交至自己的分支。直到结束工作，将自己的分支合并到原来的分支上。这样既安全，又不会影响别人工作。

Git 的分支管理与 SVN 不同，无论创建、切换和删除分支，无论版本库有多少个文件，都可以在 1 秒内完成。



## 15. 创建与合并分支

在版本回退中，每次提交，Git 都会把它们穿成一条时间线，这条时间线就是一个分支。

在 Git 里，这个分支就叫做“主分支”，即 `master` 分支。`HEAD` 严格来说指向 `master` ，而 `master` 才是指向提交的，所以 `HEAD` 指向“当前分支”。

开始时，`master` 分支是一条线，Git 用 `master` 指向最新的提交，再用 `HEAD` 指向 `master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

每次提交，`master` 分支都会向前移动一步，随着不断提交，`master` 分支的线也会越来越长。

当创建一个新的分支时，例如 `dev` 分支，Git 新建了一个指针叫做 `dev`，指向 `master` 相同的提交，再把 `HEAD` 指向 `dev`，就表示当前分支在 `dev` 上：

![img](https://static.liaoxuefeng.com/files/attachments/919022363210080/l)

```bash
$ git checkout -b dev          # 创建并切换分支
Switched to a new branch 'dev'
```

`git checkout` 加上参数 `-b` 参数表示创建并切换分支，等效于：

```bash
$ git branch dev          # 创建分支

$ git checkout dev        # 切换分支
Switched to branch 'dev'
```

然后，通过 `git branch` 命令查看现有分支情况：

```bash
$ git branch       # 列出所有分支
* dev              # 星号表示当前分支
  master
```

创建一个新的分支，仅需添加一个指针 `dev`，再改变 `HEAD` 的指向，工作区的文件没有任何变化。

从现在开始，对工作区的修改和提交就是针对 `dev` 分支了。比如新提交一次后，`dev` 指针往前移动一步，而 `master` 指针不变：

![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

例如，在 `dev` 分支上，对 `readme.txt` 做出修改：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick.      # 新添加的文字
```

然后提交：

```bash
$ git add readme.txt 

$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev` 分支的工作已经完成，切换回 `master` 分支：

```bash
$ git checkout master
Switched to branch 'master'
```

此时， `readme.txt` 中的修改消失了，因为之前的提交在 `dev` 分支上，而 `master` 分支此刻的节点并没有变：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

在 `dev` 分支完成，工作后，就可以把 `dev` 合并到 `master` 上。在 Git 中，最简单的方法就是直接把 `master` 指向 `dev` 的当前提交，完成合并：

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

```bash
$ git merge dev           # 合并指定分支到当前分支
Updating d46f35e..b17d20e
Fast-forward              # 快进模式，直接把 master 指向 dev
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

此时，`readme.txt` 中的修改内容出现了，和 `dev` 分支的最新提交完全一样。

Git 合并分支也很快，只需改动指针，工作区内容不变。

合并完 `dev` 后，可以删除 `dev` 分支，即删除 `dev` 指针。删掉后，就只剩下 `master` 一条分支：

![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)

```bash
$ git branch -d dev      # 删除分支
Deleted branch dev (was b17d20e)
```

删除 `dev` 分支后，就只剩下 `master` 分支了。

```bash
$ git branch
* master
```

因为创建、合并和删除指针都非常快，Git 鼓励用户使用分支完成某个任务，合并后在删掉分支。和直接在 `master` 分支上工作效果一样，但过程更安全。

- `switch`

  在新版本中，创建分支可以用 `git switch -c` 来代替 `git checkout -b`：

  ```bash
  $ git switch -c dev
  ```

  切换分支可以用 `git switch` 来代替 `git checkout`：

  ```bash
  $ git switch master
  ```



## 16. 解决冲突

创建一个新分支 `feature1`：

```bash
$ git switch -c feature1
Switched to a new branch 'feature1'
```

修改 `readme.txt` 的最后一行：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick AND simple.
```

在 `featuer1` 分支上提交：

```bash
$ git add readme.txt

$ git commit -m "AND simple"
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

用 `git log --graph` 命令查看当前分支情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
* dbc7f31 (HEAD -> feature1) AND simple
* 5f3a738 (origin/master, master) branch test
* f9a6701 rm test.txt
* eb7e82a test
* c469748 git tracks changes of files
* b5f02fc git tracks changes
* f1f4032 understand how stage works
* 7a3b38c add some words in readme.txt
* f04401e fix a word in readme.txt
* 8a90550 update readme.txt
```

切换到 `master` 分支：

```bash
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

此时，`readme.txt` 文件还是原来的内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick.
```

在 `master` 分支上再次修改 `readme.txt` 的最后一行：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick & simple.
```

提交：

```bash
$ git add readme.txt 

$ git commit -m "& simple"
[master 5dc6824] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`readme.txt` 和 `feature1` 分支各自都分别有新的提交：

![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

这种情况下，Git 无法执行“快速合并”，只能试图把各自的修改合并起来，但会产生冲突：

```bash
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git 告诉我们 `readme.txt` 文件发生了冲突，必须手动解决冲突后再提交。

`git status` 也会告诉我们冲突的文件：

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

查看此时的 `readme.txt` 文件的内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.    # 当前分支 master
=======
Creating a new branch is quick AND simple.  # 其他分支 feature1
>>>>>>> feature1
```

Git用 `<<<<<<<`，`=======`，`>>>>>>>` 标记出不同分支的内容，在此修改如下后保存：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple  # 在 master 分支修改
```

再提交：

```bash
$ git add readme.txt 

$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

此时的分支情况：

![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

用 `git log --graph` 命令查看当前分支情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

最后，删除 `feature1` 分支：

```bash
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

最终只有一个分支 `master`：

```bash
$ git branch
* master
```



## 17. 分支管理策略

通常， 合并分支时，Git 会用 `Fast forward` 模式，但这种模式下，删除分支后会丢掉分支信息。

如果强制禁用 `Fast forward` 模式后，Git 会在合并分支时生成一个新的 `commmit`。这样，从分支历史上可以看到分支信息。

首先，创建并切换到 `dev` 分支：

```bash
$ git switch -c dev
Switched to a new branch 'dev'
```

修改 `readme.txt` 文件，并提交：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
Testing new merge method.
```

```bash
$ git add readme.txt 

$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，切换回 `master`：

```bash
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

准备与 `dev` 分支合并，参数采用 `--no-ff`，表示禁用 `Fast forward` 模式：

```bash
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

本次合并分支创建了一个新的 `commit`，所以加上 `-m` 参数，用来描述内容。

查看分支历史：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

可以看到，采用 `--no-ff` 参数后，合并后的路径图发生变化：

![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

禁用 `Fast forward` 模式后，会在合并时创建一个新的节点，附有新的 `commit` 信息。

使用 `Fast forward` 时：

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

两个分支共用一个节点，`dev` 分支原来的 `commit` 信息被覆盖。

- 分支策略

  在实际开发中，团队成员应该共同遵守一些分支规则：

  `master` 分支应保持稳定，仅用来发布最新版本，平时尽量减少在该分支的修改；

  `dev` 分支是不稳定的，用来进行修改、更新或删除等工作。等到在该分支上实现阶段性更新后，再将内容推送至 `master` 分支；

  团队中的每个成员都应该在 `dev` 分支上建立自己的分支，只要定期往 `dev` 分支上合并起来就可以了。

  ![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)



## 18. Bug 分支

在软件开发中，每个 bug 都可以用过一个新的临时分支来修复。修复后，合并分支，再删除该临时分支。

当遇到一个代号 101 的 bug 任务时，需要创建一个 `issur-101` 分支来修复它。但是，当前 `dev` 分支上进行的工作还没有提交：

```bash
$ git status
On branch dev
Changes to be committed:        # 创建了一个新的文件，添加至暂存区，但尚未提交
  (use "git restore --staged <file>..." to unstage)
        new file:   hello.py

Changes not staged for commit:  # 修改了 readme 文件，但尚未添加至暂存区
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt
```

Git 提供了一个 `stash` 功能，可以把当前工作现场“储藏”起来，可以在之后恢复现场，再继续工作：

```bash
$ git stash    # 用于临时保存和恢复修改，可跨分支
Saved working directory and index state WIP on dev: 9d0f0f9 merge with no-ff
```

现在，用 `git status` 查看工作区：

```bash
$ git status
On branch dev
nothing to commit, working tree clean
```

此时，`dev` 分支没有未添加或未提交的文件，因为上述修改已经被 `stash` 储藏起来。

假设需要在 `master` 分支上修复 bug，就从 `master` 上创建临时分支：

```bash
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git switch -c issue-101
Switched to a new branch 'issue-101'
```

此时的分支情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   9d0f0f9 (HEAD -> issue-101, origin/master, master, dev) merge with no-ff
|\
| * ab781c0 add merge
|/
* a4f66df test
*   9a0f5c4 conflict fixed
...
```

现在需要修复 bug，需要把 “Git is free software…” 修改为 “Git is a free software…”：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is a free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
Testing new merge method.

$ git add readme.txt 

$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到 `master` 分支，合并 `issue-101` 分支，最后删除 `issue-101` 分支：

```bash
$ git merge --no-ff -m "mergerd bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

当前分支情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   3be0b9e (HEAD -> master) mergerd bug fix 101
|\
| * b241e17 (issue-101) fix bug 101
|/
*   9d0f0f9 (origin/master, dev) merge with no-ff
|\
| * ab781c0 add merge
|/
* a4f66df test
...
```

删除 `issue-101` 分支：

```bash
$ git branch -d issue-101
Deleted branch issue-101 (was b241e17).
```

现在需要恢复 `dev` 分支的工作，首先查看当前的 `stash list`：

```bash
$ git stash list
stash@{0}: WIP on dev: 9d0f0f9 merge with no-ff
```

恢复工作现场有两种方式：

- `git stash apply` 恢复后，用 `git stash drop` 删除；
- 直接用 `git stash pop` 恢复并删除。

```bash
$ git stash pop
Auto-merging readme.txt
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

Dropped refs/stash@{0} (682cbf4f91b7924c25f19f27af30e48bbd98878c)
```

再次查看 `stash lsit`，显示没有任何内容：

```bash
$ git stash list
```

可以多次stash，恢复的时候，先用 `git stash list` 查看，然后恢复指定的 `stash`，用命令：

```bash
$ git stash apply stash@{0}
```

同样的 bug，在分支 `dev` 上修复，只需要把  `b241e17 fix bug 101` 这个提交所作的修改“复制”到 `dev` 分支，并不是与整个 `master` 分支合并。

Git 使用 `cherry-pick` 命令，复制一个特定的提交到当前分支：

```bash
$ git cherry-pick b241e17
[dev 589519d] fix bug 101
 Date: Wed Jul 1 09:54:06 2020 +0800
 1 file changed, 1 insertion(+), 1 deletion(-)
```

`cherry-pick` 命令自动生成一个 `commit` ，id 是 `589519d`，不同于 `master` 的 `3be0b9e`。因为虽然这两次提交的改动相同，但却是不同的提交。



## 19. Feature 分支

在软件开发过程中，有时需要添加一个新的功能。为了不影响主分支的稳定性，每添加一个功能，最好新建一个 `feature` 分支。在 `feature` 分支上进行开发和合并，最后再删除该分支。

例如，现在需要开发一个代号为 `Vulcan` 的新功能，用于下一代星际飞船：

```bash
$ git switch -c feature-vulcan
Switched to a new branch 'feature-vulcan'

$ vi vulcan.py
```

开发完成后提交：

```bash
$ git add vulcan.py

$ git commit -m "add feature vulcan"
[feature-vulcan 287773e] add feature vulcan
 1 file changed, 2 insertions(+)
 create mode 100644 vulcan.py
```

切回 `dev`，准备合并：

```bash
$ git switch dev
```

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

就在此时，接到上级命令，因经费不足，新功能必须取消！

删除分支 `feature-vulcan`：

```bash
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

销毁失败。Git 提醒，`feature-vulcan` 分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。

强行删除：

```bash
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

删除成功！



## 20. 多人协作

当从远程仓库克隆时，实际上 Git 自动把本地的 `master` 分支和远程的 `master` 分支对应起来了。并且，远程仓库的默认名称是 `origin`。

要查看远程库的信息，用 `git remote` 或 `git remote -v`：

```bash
$ git remote
origin

$ git remote -v      # 查看详细信息
origin  git@github.com:Jeff1999/learngit.git (fetch)
origin  git@github.com:Jeff1999/learngit.git (push)
```

结果显示，远程仓库 `origin` 可以从本地抓取和推送。如果没有推送权限，就看不到 `push` 的地址。

- 推送分支

  推送分支，即将本地分支上的所有提交都推送到远程库。推送时，要指定本地分支的名称。

  ```bash
  $ git push origin master   # 推送本地的 master 分支
  
  $ git push origin dev      # 推送本地的 dev 分支
  ```

  - `master` 分支是主分支，要时刻与远程库保持同步；

  - `dev` 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

  - bug 分支只用于在本地修复 bug，没有必要推送到远程；
  - feature 分支是否推送到远程，取决于是否和他人合作开发。

- 抓取分支

  多人协作时，大家都会向 `master` 和 `dev` 分支上推送各自的修改。

  现在模拟另一个合作开发的同伴，可以在另一台电脑（注意要把 SSH Key 添加到 GitHub），或者在同一台电脑的另一个目录下克隆：

  ```bash
  $ git clone git@github.com:Jeff1999/learngit.git
  Cloning into 'learngit'...
  remote: Enumerating objects: 58, done.
  remote: Counting objects: 100% (58/58), done.
  remote: Compressing objects: 100% (32/32), done.
  remote: Total 58 (delta 19), reused 58 (delta 19), pack-reused 0
  Receiving objects: 100% (58/58), 5.08 KiB | 1.01 MiB/s, done.
  Resolving deltas: 100% (19/19), done.
  ```

  当另一个本地仓库从远程库克隆时，默认情况下只能看到远程仓库的 `master` 分支：

  ```bash
  $ git branch
  * master
  ```

  如果要在 `dev` 分支上开发，就必须克隆远程的 `dev` 分支到该本地仓库：

  ```bash
  $ git checkout -b dev origin/dev
  ```

  现在，对方就可以在 `dev` 上继续修改，然后时不时地把 `dev` 分支 `push` 到远程：

  ```bash
  $ git add env.txt
  
  $ git commit -m "add env"
  [dev 7a5e5dd] add env
   1 file changed, 1 insertion(+)
   create mode 100644 env.txt
  
  $ git push origin dev
  Counting objects: 3, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
     f52c633..7a5e5dd  dev -> dev
  ```

  此时，对方已经向 `origin/dev` 分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：、

  ```bash
  $ cat env.txt
  env
  
  $ git add env.txt
  
  $ git commit -m "add new env"
  [dev 7bd91f1] add new env
   1 file changed, 1 insertion(+)
   create mode 100644 env.txt
  
  $ git push origin dev
  To github.com:michaelliao/learngit.git
   ! [rejected]        dev -> dev (non-fast-forward)
  error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
  hint: Updates were rejected because the tip of your current branch is behind
  hint: its remote counterpart. Integrate the remote changes (e.g.
  hint: 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```

  推送失败，因为对方的最新提交和你试图推送的提交有冲突，解决办法也很简单， Git 已经提示我们，先用 `git pull` 把最新的提交从 `origin/dev` 下来，然后，在本地合并并解决冲突，再推送：

  ```bash
  $ git pull
  There is no tracking information for the current branch.
  Please specify which branch you want to merge with.
  See git-pull(1) for details.
  
      git pull <remote> <branch>
  
  If you wish to set tracking information for this branch you can do so with:
  
      git branch --set-upstream-to=origin/<branch> dev
  ```

  `git pull` 也失败了，原因是没有指定本地 `dev` 分支与远程 `origin/dev` 分支的链接，根据提示，设置 `dev` 和 `origin/dev` 的链接：

  ```bash
  $ git branch --set-upstream-to=origin/dev dev
  Branch 'dev' set up to track remote branch 'dev' from 'origin'.
  ```

  再 pull：

  ```bash
  $ git pull
  Auto-merging env.txt
  CONFLICT (add/add): Merge conflict in env.txt
  Automatic merge failed; fix conflicts and then commit the result.
  ```

  这回 `git pull` 成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后提交，再 push：

  ```bash
  $ git commit -m "fix env conflict"
  [dev 57c53ab] fix env conflict
  
  $ git push origin dev
  Counting objects: 6, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (4/4), done.
  Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
  Total 6 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
     7a5e5dd..57c53ab  dev -> dev
  ```

  因此，多人协作的工作模式通常是这样：

  1. 首先，可以试图用 `git push origin <branch-name>` 推送自己的修改；
  2. 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功！

  如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream-to <branch-name> origin/<branch-name>` 创建链接关系。

