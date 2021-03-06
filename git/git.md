# 常见git命令

> origin：仓库名称 master：分支

- 创建git仓库

`git init`

- 添加文件到仓库

`git add <file>`

- 提交文件到仓库 -m：提交说明

`git commit -m ""`

- 显示当前仓库状态

`git status`

- 显示差异

`git diff`

- 显示file文件的差异

`git diff HEAD -- <file>`

- 显示日志 --pretty=oneline：每个记录显示一行，包含id和说明

`git log`

- 回到指定版本

`git reset --hard commit_id`

- 记录每一次命令

`git reflog`

- 撤销工作区file文件的修改

`git checkout -- <file>`

- 撤销暂存区的修改，并放回工作区

`git reset HEAD <file>`

- 删除文件

`git rm <file>`

- 创建SSH Key，在.ssh目录里面生成id_rsa（公钥）和id_rsa.pub（私钥）

`ssh-keygen -t rsa -C "yuanxiaowa@qq.com"`

- 关联远程仓库

`git remote add origin <url>`

- 将本地内容推送到远程 -u：把本地的master分支和远程的master分支关联起来（第一次推送使用）

`git push origin master`

- 克隆一个本地库

`git clone <url>`

- 创建dev分支

`git branch dev`

- 切换到dev分支

`git checkout dev`

- 创建dev分支，然后切换到dev分支

`git checkout -b dev`

- 查看当前分支

`git branch`

- 把dev分支的工作成果合并到当前分支上

`git merge dev`

- 删除本地dev分支

`git branch -d dev`

- 强制删除dev分支

`git branch -D dev`

- 删除远程分支

`git push origin :dev`

- 查看分支合并状况 --graph：查看分支合并图

`git log --graph --pretty=oneline --abbrev-commit`

- 合并 --no-ff：禁用Fast forward模式 并生成一个新的commit，这样可以在分支历史上看出分支合并信息

`git merge --no-ff -m "" dev`

- 保存工作现场

`git stash`

- stash列表

`git stash list`

- 恢复工作现场，stash内容不删除，需要使用git stash drop来删除 --stash@{0}：恢复到指定的stash

`git stash apply [stash@{0}]`

- 恢复工作现场，恢复的同时把stash内容也删除

`git stash pop`

- 查看远程库的信息

`git remote`

- 查看远程库更详细的信息

`git remote -v`

- 创建本地dev分支并与远程的dev分支链接

`git checkout -b dev origin/dev`

- 拉取最新的提交

`git pull`

- 将本地的dev分支与远程的dev分支链接

`git branch --set-upstream dev origin/dev`

- 打标签,名字为name

`git tag <name>`

- 查看所有标签

`git tag`

- 在id处打标签

`git tag <name> <id>`

- 查看name标签信息

`git show <name>`

- 创建带说明的标签 -a：标签名 -m：说明文字

`git tag -a <name> -m ""`

- 通过-s用私钥签名一个标签，签名采用PGP签名，因此，必须首先安装gpg，如果没有找到gpg，或者没有gpg密钥对，就会报错

`git tag -s <name> -m "" <id>`

- 删除标签

`git tag -d <name>`

- 推送某个标签到远程

`git push origin <name>`

- 推送全部尚未推送标签到远程

`git push origin --tags`

- 远程删除标签

`git push origin :refs/tags/<name>`
> 如果标签已经推送到远程，先从本地删除，在远程删除

每个仓库的配置文件放在.git/config中，全局的在家目录的隐藏文件.gitconfig中
```shell
git config
  --global
  #加上会对当前用户起作用，不加只对当前仓库起作用
    user.name
    user.email
    color.ui true：适当的显示不同的颜色
```
  
## 忽略文件  
在根目录建一个.gitignore文件，把要忽略的文件名填进去，git可以自动忽略这些文件
- `*.db x.html`
- `bin/`

## 配置别名
```shell
git config
  --global
    alias.st status
    #使用git st代替git status
    alias.unstage 'reset HEAD'
    #使用git unstage代替git reset HEAD
    alias.last 'log -1'
    #使用git last代替git log -1，表示显示最后一次提交信息
    alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
  
## 搭建git服务器
__一、安装git__

1. `apt-get install git`
2. `yum install git`

__二、创建git用户，用来运行git服务__

`adduser username`

__三、创建证书登录__

收集用户的公钥，导入~/git/.ssh/authorized_keys

__四、初始化git仓库__

1. `git init --bare sample.git`
> 仓库为sample.git文件夹

2. chown -R git:username sample.git
> 把owner改为username

__五、禁用shell登录__

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

`git:x:1001:1001:,,,:/home/git:/bin/bash`

改为

`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

## 管理公钥
使用工具Gitosis

## 管理权限
使用Gitolite