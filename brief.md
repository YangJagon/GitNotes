```shell
# 安装完git后首先需要设置用户名和邮箱
$ git config --global user.name "John Doe" 
$ git config --global user.email johndoe@example.com

# 列出所有 Git 当时能找到的配置
$ git config --list	
user.name=John Doe 
user.email=johndoe@example.com 
...

# 查看某一项的设置
$ git config user.name	
John Doe

# 获取帮助
$ git help <verb> 

# 初始化仓库
$ git init

# 克隆一个仓库到本地
$ git clone https://github.com/libgit2/libgit2
# 与上一命令相同，只不过在本地的仓库名字改为"mylibgit"
$ git clone https://github.com/libgit2/libgit2 mylibgit

# 查看当前仓库状态
$ git status

# 追踪新文件
$ git add *

# 删除文件
$ git rm README

# 让文件保留在磁盘，但是并不想让 Git 继续跟踪
$ git rm --cached README

# 移动文件
$ git mv file_from file_to

# 查看尚未暂存的文件更新了哪些部分
$ git diff
# 查看已暂存的将要添加到下次提交里的内容
$ git diff --cached 

# 提交文件
$ git commit -m "Story 182: Fix benchmarks for speed"

# 查看提交历史
$ git log
# 加 -p 显示每次提交的内容差异，加上-2来仅显示最近两次提交
$ git log -p -2
# 加 --stat 显示每次提交的简略的统计信息
$ git log --stat

# 重新提交
$ git commit --amend

# 取消暂存文件
$ git reset HEAD CONTRIBUTING.md

# 撤销更改
$ git checkout -- CONTRIBUTING.md

# 查看远程仓库信息
$ git remote
origin
# -v会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL
$ git remote -v 
# 要查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。
$ git remote show origin

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

#访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
$ git fetch [remote-name]

# 将master分支推送到origin服务器
$ git push origin master

# 列出标签
$ git tag
v0.1
v1.3

# 如果只对1.8.5系列感兴趣
$ git tag -l 'v1.8.5*'
v1.8.5
v1.8.5-rc0

# 添加一个附注标签
$ git tag -a v1.4 -m 'my version 1.4'

# 添加一个轻量标签
$ git tag v1.4-lw

# 后期添加标签
$ git log --pretty=oneline
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
# 在命令的末尾指定提交的校验和（或部分校验和）:
$ git tag -a v1.2 9fceb02

# 推送标签到origin服务器上
$ git push origin v1.5

# 一次性推送多个标签
$ git push origin --tags

# 根据标签创建一个新的分支
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'

# 定义别名，使“git unstage fileA”和“git reset HEAD -- fileA”等价
$ git config --global alias.unstage 'reset HEAD --'

# 查看分支
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the

# 创建分支
$ git branch testing 

# 切换分支
$ git checkout testing 

# 创建并切换到新分支
$ git checkout -b testing 

# 将分支testing合并到master中
$ git checkout master
Switched to branch 'master'
$ git merge testing

# 将experiment的修改变基到master上
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command

# 将server中的修改变基到master上
$ git rebase master server

# 取出client分支，找出处于client分支和server分支的共同祖先之后的修改，然后把它们在master分支上重放一遍
$ git rebase --onto master server client

# 删除分支
$ git branch -d testing

# 查看分支列表：
$ git branch
  iss53
* master
  testing
  
# 查看每一个分支的最后一次提交
$ git branch -v
  iss53 93b412c fix javascript issue
* master 7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes

# 列出分支列表中已经合并或尚未合并到当前分支的分支。
$ git branch --merged	#查看哪些分支已经合并到当前分支
  iss53
* master
$ git branch --no-merged #查看所有包含未合并工作的分支
  testing
  
# 无法正常删除还未合并的分支
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.

# 查看远程服务器的分支
$ git ls-remote (remote)
$ git remote show (remote)

# 创建一个跟踪对应远程分支的本地分支
# 相当于 git checkout --track origin/serverfix
$ git checkout -b serverfix teamone/serverfix
Branch serverfix set up to track remote branch serverfix from teamone.
Switched to a new branch 'serverfix'

# 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.

# 推送本地的serverfix分支来更新远程仓库上的serverfix分支。
$ git push origin serverfix

# 将本地的serverfix分支推送到远程仓库上的awesomebranch分支。
$ git push origin serverfix:awesomebranch 

# 删除远程分支
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted] serverfix
```

