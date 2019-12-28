# 基础命令

Git 有三种状态，已提交（committed）、已修改（modified）和已暂存（staged）。 

 Git 项目的三个工作区域的概念：Git 仓库、工作目录和暂存区域。

## git文件生命周期图

![image-20191229104544176](.\image\image-20191228152121575.png)

## git 初始设置以及查看和帮助命令

```shell
# 安装完git后首先需要设置用户名和邮箱
$ git config --global user.name "John Doe" 
$ git config --global user.email johndoe@example.com

# 列出所有 Git 当时能找到的配置
$ git config --list	
user.name=John Doe 
user.email=johndoe@example.com 
color.status=auto 
...

# 查看某一项的设置
$ git config user.name	
John Doe

# 获取帮助，如 git help config
$ git help <verb> 
$ git <verb> --help 
$ man git-<verb>
```

## 初始git仓库命令

```shell
# 该命令将创建一个名为.git的子目录，这个子目录含有初始化Git仓库中所有的必须文件
$ git init

# 克隆一个仓库到本地
$ git clone https://github.com/libgit2/libgit2
# 与上一命令相同，只不过在本地的仓库名字改为"mylibgit"
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

## 操作git仓库命令

```shell
# 查看当前仓库状态
$ git status

# 追踪新文件
$ git add README

# 删除文件
$ git rm README

# 让文件保留在磁盘，但是并不想让 Git 继续跟踪
$ git rm --cached README

# 移动文件
$ git mv file_from file_to
```

## 忽略文件

我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式

```shell
$ cat .gitignore 
# no .a files 
*.a

# but do track lib.a, even though you're ignoring .a files above 
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO 
/TODO

# ignore all files in the build/ directory 
build/

# ignore doc/notes.txt, but not doc/server/arch.txt 
doc/*.txt

# ignore all .pdf files in the doc/ directory 
doc/**/*.pdf
```

规范如下：

1. 所有空行或者以 ＃ 开头的行都会被 Git 忽略。 
2. 可以使用标准的 glob 模式匹配。 
3. 匹配模式可以以（/）开头防止递归。 
4. 匹配模式可以以（/）结尾指定目录。 
5. 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反

## 查看已暂存和未暂存的修改 

```shell
$ git diff	#查看尚未暂存的文件更新了哪些部分
$ git diff --cached #查看已暂存的将要添加到下次提交里的内容
```

## 提交更新

```shell
$ git commit
$ git commit -m "Story 182: Fix benchmarks for speed"
```

## 查看提交历史

```shell
# 查看提交历史
$ git log

# 加 -p 显示每次提交的内容差异，加上-2来仅显示最近两次提交
$ git log -p -2

# 加 --stat 显示每次提交的简略的统计信息
$ git log --stat

# 加 --pretty 可以指定使用不同于默认格式的方式展示提交历史,还有short, full, fuller, format
$ git log --pretty=oneline
$ git log --pretty=format:"%h - %an, %ar : %s" --graph #加graph可以更形象展示
# 还有更多限制查阅《Pro Git》
```

## 撤消操作

### 重新提交

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 **--amend** 选项的提交命令尝试重新提交：

```shell
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

### 取消暂存文件

使用 **git reset HEAD <fileName>** 取消暂存文件

```shell
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
  renamed: README.md -> README
  modified: CONTRIBUTING.md
$ git reset HEAD CONTRIBUTING.md
```

### 撤消对文件的修改

使用 **git  checkout --**，对文件做的任何修改都会消失，相当于只是拷贝了另一个文件来覆盖它。

```shell
$ git status
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working
directory)
  modified: CONTRIBUTING.md
$ git checkout -- CONTRIBUTING.md
```

## 远程仓库的使用

### 查看远程仓库

```shell
# 使用 git remote 查看远程仓库信息，origin是Git给克隆的仓库服务器的默认名字
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
$ cd ticgit
$ git remote
origin

$ git remote -v # -v会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL

# 要查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。
$ git remote show origin
```

### 添加、移除与重命名远程仓库

```shell
# 添加一个新的远程 Git 仓库
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote
origin
pb

# 重命名引用的名字
$ git remote rename pb paul
$ git remote
origin
paul

# 移除一个远程仓库
$ git remote rm paul
$ git remote
origin
```

### 抓取和推送远程仓库

```shell
#访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
$ git fetch [remote-name]

# 将master分支推送到origin服务器
$ git push origin master
```

## 打标签

Git 可以给历史中的某一个提交打上标签，以示重要。

```shell
# 列出标签
$ git tag
v0.1
v1.3

# 如果只对1.8.5系列感兴趣
$ git tag -l 'v1.8.5*'
v1.8.5
v1.8.5-rc0
```

标签分为**轻量标签**（lightweight）与**附注标签**（annotated）。一个**轻量标签**很像一个不会改变的分支，它只是一个特定提交的引用。**附注标签**是存储在 Git 数据库中的一个完整对象。

```shell
# 添加一个附注标签
$ git tag -a v1.4 -m 'my version 1.4'

# 添加一个轻量标签
$ git tag v1.4-lw
```

Git可以后期打标签

```shell
$ git log --pretty=oneline
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

# 在命令的末尾指定提交的校验和（或部分校验和）:
$ git tag -a v1.2 9fceb02
```

默认情况下，git push 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。

```shell
# 推送标签到origin服务器上
$ git push origin v1.5

# 一次性推送多个标签
$ git push origin --tags
```

在 Git 中并不能真的检出一个标签，因为它们并不能像分支一样来回移动。如果想要工作目录与仓库中特定的标签版本完全一样，可以使用 **git checkout -b [branchname] [tagname]** 在特定的标签上创建一个新分支：

```shell
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

当然，如果在这之后又进行了一次提交，version2 分支会因为改动向前移动了，那么 version2 分支就会和 v2.0.0 标签稍微有些不同，这时就应该当心了。

## Git 别名

可以通过 **git config** 文件来轻松地为每一个命令设置一个别名。 

```shell
# 使 “git unstage fileA” 和 “git reset HEAD -- fileA” 等价
$ git config --global alias.unstage 'reset HEAD --'
```

# 分支

## 查看分支

Git 有一个名为 HEAD 的特殊指针，指向当前所在的本地分支。可以简单地使用 **git log** 命令查看各个分支当前所指的对象。 提供这一功能的参数是 **--decorate**。

```shell
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

## 创建分支

```shell
$ git branch testing 
```

## 切换分支

```shell
$ git checkout testing 

#若要新建一个分支并同时切换到新分支，可以在checkout后面加 -b
$ git checkout -b testing 
```

## 合并分支

```shell
# 若想将分支testing合并到master中
$ git checkout master
Switched to branch 'master'
$ git merge testing
```

##  删除分支

```shell
$ git branch -d testing
```

## 分支冲突

 产生合并冲突时，Git会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 git status 命令来查看那些因包含合并冲突而处于未合并 （unmerged）状态的文件： 

```shell
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
Unmerged paths:
  (use "git add <file>..." to mark resolution)
  both modified: index.html
no changes added to commit (use "git add" and/or "git commit -a"
```

## 分支管理

```shell
# git branch 不加任何参数运行它，会得到当前所有分支的一个列表：
$ git branch
  iss53
* master
  testing
  
# 如果需要查看每一个分支的最后一次提交，可以运行 git branch -v 命令：
$ git branch -v
  iss53 93b412c fix javascript issue
* master 7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes

# --merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。
$ git branch --merged	#查看哪些分支已经合并到当前分支
  iss53
* master

$ git branch --no-merged #查看所有包含未合并工作的分支
  testing
  
# 由于此时testing分支还未合并，所以无法正常删除
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
```

## 查看远程分支

远程引用是对远程仓库的引用（指针），包括分支、标签等等。你可以通过 **git ls-remote (remote)** 来 显式地获得远程引用的完整列表，或者通过 **git remote show (remote)** 获得远程分支的更多信息。

```shell
$ git ls-remote (remote)
$ git remote show (remote)
```

## 抓取远程分支

当 git fetch 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然 后让你自己合并。 然而，有一个命令叫作 git pull 在大多数情况下它的含义是一个 git fetch 紧接着一个 git merge 命令。

```shell
$ git fetch teamone
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/schacon/simplegit
 * [new branch] serverfix -> teamone/serverfix
```

## 跟踪远程分支

当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。可以运行 **git merge teamone/serverfix** 将这些工作合并到当前所在的分支。 如果想要在自己的 serverfix 分支上工作，可以将其建立在远程跟踪分支之上：

```shell
# 相当于 git checkout --track origin/serverfix
$ git checkout -b serverfix teamone/serverfix
Branch serverfix set up to track remote branch serverfix from teamone.
Switched to a new branch 'serverfix'

# 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

## 推送远程分支

```shell
# 推送本地的serverfix分支来更新远程仓库上的serverfix分支。
$ git push origin serverfix

# 将本地的serverfix分支推送到远程仓库上的awesomebranch分支。
$ git push origin serverfix:awesomebranch 
```

## 删除远程分支

可以运行带有 **--delete** 选项的 **git push** 命令来删 除一个远程分支。 如果想要从服务器上删除 serverfix 分支，运行下面的命令：

```shell
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted] serverfix
```

