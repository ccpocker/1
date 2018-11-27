---
layout: post
title:  Git常用命令整理-Git基础
categories: Tools
tags: Git
author: ccpocker
---

* content
{:toc}

### 初次运行Git的前的配置

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：


**用户信息**
当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：
```bash
$ git config --global user.name "John Doe" 
$ git config --global user.email johndoe@example.com
```
**文本编辑器**
```bash
$ git config --global core.editor emacs
```
**检查配置信息**
```bash
$ git config --list
```
### Git基础

#### 2.1 Git 基础 - 获取 Git 仓库
 **在现有目录中初始化仓库**
```bash
$ git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。 


如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。 你可通过 git add 命令来实现对指定文件的跟踪，然后执行 git commit 提交：
```bash
$ git add *.c  
$ git add LICENSE  
$ git commit -m 'initial project version'
```
 **克隆现有的仓库**

 如果你想获得一份已经存在了的 Git 仓库的拷贝，比如说，你想为某个开源项目贡献自己的一份力，这时就要用到git clone 命令。
 格式： git clone [url]
```bash
$ git clone https://github.com/libgit2/libgit2
```

#### 2.2 Git 基础 - 记录每次更新到仓库

请记住，你工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。


**检查当前文件状态**
```bash
$ git status
$ git status -s #状态简览
```
**跟踪新文件**
```bash
$ git add 
```

**查看已暂存和未暂存的修改**
要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 git diff：
请注意，git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。
然后用 git diff --cached 查看已经暂存起来的变化：（--staged 和 --cached 是同义词）
```bash
$ git diff  #看暂存前后的变化：
git diff --cached #查看已经暂存起来的变化
```
**提交更新**
```bash
$ git commit #提交更新
```
**移除文件**
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

**移动文件**
既然如此，当你看到 Git 的 mv 命令时一定会困惑不已。 要在 Git 中对文件改名，可以这么做：
```bash
$ git mv file_from file_to
```
其实，运行 git mv 就相当于运行了下面三条命令：
```bash
$ mv README.md README
$ git rm README.md
$ git add README
```

#### 2.3 Git 基础 - 查看提交历史

***查看提交历史**
```bash
$ git log 
```
一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交.
如果你想看到每次提交的简略的统计信息，你可以使用 --stat 选项：


#### 2.4 Git 基础 - 撤消操作
**撤消操作**

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令尝试重新提交：
```bash
$ git commit --amend
```
例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：
```bash
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

**取消暂存的文件**
演示如何操作暂存区域与工作目录中已修改的文件。 这些命令在修改文件状态的同时，也会提示如何撤消操作。 例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 git add * 暂存了它们两个。 

使用 git reset HEAD <file>... 来取消暂存

**撤消对文件的修改**

git checkout -- <file>...

#### 2.5 Git 基础 - 远程仓库的使用

**查看远程仓库**
```bash
$ git remote 
$ git remote -v #指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。
```
**添加远程仓库**
```bash
git remote add <shortname> <url> #添加远程仓库 shortname:简称 url:地址
```
现在你可以在命令行中使用字符串 pb 来代替整个 URL。
如果你想拉取 Paul 的仓库中有但你没有的信息，可以运行 git fetch pb

**从远程仓库中抓取与拉取**
```bash
$ git fetch [remote-name] #从远程仓库中获得数据,这个命令会访问远程仓库，从中拉取所有你还没有的数据。
```
必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

**推送到远程仓库**
git push [remote-name] [branch-name]

$ git push origin master 

只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

**查看远程仓库**
如果想要查看某一个远程仓库的更多信息，可以使用
git remote show [remote-name] 命令。

**远程仓库的移除与重命名**
如果想要重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。 
如果因为一些原因想要移除一个远程仓库 - 你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了 - 可以使用 git remote rm 。
```bash
$ git remote rename pb paul #改名
$ git remote rm paul  #删除远程仓库
```

#### 2.6 Git 基础 - 打标签

**列出标签**
Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。
```bash
$ git tag
$ git tag -a v1.4 -m 'my version 1.4' #添加附注标签，-m 选项指定了一条将会存储在标签中的信息。 
$ git tag v1.4-lw #添加轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字
$ git show [tagname] #显示标签信息
```

**共享标签**
默认情况下，git push 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样 
```bash
$ git push origin v1.5 #推送标签v1.5到远程服务器
$ git push origin --tags #推送所有不在远程服务器的标签推送
```

##2.7 Git 基础 - Git 别名
```bash
$ git config --global alias.co checkout #将checkout改为co
$ git config --global alias.br branch  #将branch改为br
$ git config --global alias.ci commit
$ git config --global alias.st status

$ git config --global alias.last 'log -1 HEAD' # 组合改名
```