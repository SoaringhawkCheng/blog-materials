---
title: Git学习笔记
date: 2016-12-16 18:39:44
categories:
- 编程
tags:
- Git
---

如果你因为发现人类社会是如此的不尽如意，而感到自己更加适合遁入孤独，那么你就是一个不能长期忍受孤独的沉闷压抑的人，尤其在你年轻的时候。对此，我建议你养成这样的习惯：把部分孤独带入社会交往中，学会在人群中保持一定程度的孤独，不要立即说出自己的想法，也不要太过在意别人所说的话；勿对别人有太多的期待，无论是道德还是才智上；对于他人的看法，应加强锻炼自己无动于衷的冷漠态度和感觉---这是培养值得称道的宽容品质的一个最切实可靠的方法。 
假如，你做到了这些，那么即使你生活在众人当中，也不会与他人有过多的联系和交往：你和他们的关系将是纯粹客观的。这一预防措施将使你与社会保持必要的距离，不至于离得太近，而且也能保护你与社会保持必要的距离。
						————亚瑟·叔本华
				
<!--more-->
## 集中式vs分布式
集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。

分布式版本控制系统的安全性要高很多。因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

## 常用命令行

### 安装Git

Linux

```
$ sudo apt-get install git
```

Macos

```
$ brew cask install git
```

### 配置Git

```
$ git config --global user.name "Soainghawk Cheng"
$ git config --global user.email "2856043766@qq.com"
```

### 创建Git仓库

```
$ git init
```
### 工作区&暂存区

* 工作区就是你在电脑里能看到的目录。
* 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
 
![](https://github.com/SoaringhawkCheng/markdown/blob/master/learn/linux/git/0.jpg?raw=true)

### 添加文件到Git仓库
* 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区(stage)；

![](https://github.com/SoaringhawkCheng/markdown/blob/master/learn/linux/git/1.jpg?raw=true)

* 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

![](https://github.com/SoaringhawkCheng/markdown/blob/master/learn/linux/git/2.jpg?raw=true)

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

 
```
$ git commit -m '...'
```

### 仓库的状态
```
$ git status
```
### 查看Git
```
$ git diff
```
### 撤销修改
丢弃工作区修改

```
$ git checkout -- <file>
```

撤销暂存区的修改

```
$ git reset HEAD <file>
```
### 删除文件
删除工作区文件

```
$ rm <file>
```

删除版本库文件

```
$ git rm test.txt
$ git commit -m '...'
```
从版本库恢复文件

```
$ git checkout -- <file>
```
### 添加远程库
* 创建SSH Key

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key:

```
$ ssh-keygen -t rsa -C "2856043766@qq.com"
```

然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

* 登陆GitHub

打开“Account settings”，“SSH Keys”页面。然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。Macos无法直接打开`id_rsa.pub`,可以在终端输入：

```
$ more id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDI1d9ac4WEhAeO8YqaBF1nVEtnZS2lDUOxXmz/4giJtAh1BfFkUvMTN6oPAZ1dqyzMXM+w4vzyyujQaHyu2tApJTt0/oJb+Fz3z5oUOxvDdfi4m9X255ELAlRXaxQ4EeCoqSLOmaTibePUOgiWNE+SGVP87nB3zRzN9uiVbfq4M42EL1Pmd41s666pm3bk4bYy++ANiuvlsCK3yw8J/HUcg83ABZGdKURkI6B73U1lhsYRvqc+tcCrOp07JbqWSfbeJq09YhsB38Tmj+ZC9+AwGhZLdAwiyQR30RsFF9s4aAOzPftBbHbI6ZcY9EfmxgFbunw/pNOR+a1XYrMG0s7x 2856043766@qq.com
```

* 同步Git仓库

在本地创建了一个Git仓库后，又在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步：

```
$ git remote add origin git@github.com:<username>/*.git
```

第一次推送：

```
$ git push -u origin master
```
以后再推送：

```
$ git push origin master
```
把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。
由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。
### 从远程库克隆
```
$ git clone git@github.com:username/*.git
```
### 版本信息
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭.

![](https://github.com/SoaringhawkCheng/markdown/blob/master/learn/linux/git/3.jpg?raw=true)

退回上个版本：

```
$ git reset --hard HEAD^
```
退回历史版本：

```
git reset --hard commit_id。
```

穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数.

### 分支管理
查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

查看分支图：

```
$ git log --graph --pretty=oneline --abbrev-commit
```

强制禁用`Fast forward`模式:

```
$ git merge --no-ff -m '...' <branch>
```
Git就会在merge时生成一个新的`commit 6224937`，这样，从分支历史上就可以看出分支信息。

```
$ git log --graph --pretty=oneline --abbrev-commit
*   7825a50 merge with no-ff
|\
| * 6224937 add merge
|/
*   59bc1cb conflict fixed
```

### 特殊分支
* BUG分支

当你接到一个修复一个bug的任务时，很自然地，你想创建一个分支来修复它，但是，等等，当前正在dev上进行的工作还没有提交。并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
```
查看存储的工作现场：

```
$ git stash list

```
恢复工作现场，有三个办法：

1. 用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，需要用`git stash drop`来删除；
2. 用`git stash pop`，恢复的同时把`stash`内容也删了。

你可以多次`stash`，恢复的时候，先用`git stash list`查看，然后恢复指定的`stash`，用命令：

```
$ git stash apply stash@{0}
```

* Feature分支

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

如果新功能必须取消，这个分支必须要就地销毁：

```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```
销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改.如果要强行删除，需要使用命令:
```
$ git branch -D feature-vulcan。
```

### 多人协作
查看远程库信息:`git remote`，加上`-v`显示更多信息；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

因此，多人协作的工作模式通常是这样：

首先，可以试图用`git push origin branch-name`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

### 标签管理
在最新提交的commit上打标签：

```
$ git tag <tagname>
```

对指定的commit id打上标签：

```
$ git tag <tagname> <commitid>
```

查看所有标签：

```
$ git tag
```

查看具体标签信息：

```
$ git show <tagname>
```
创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：


```
$ git tag -a <tagname> -m '<versionname>' <commitid>
```
通过-s用私钥签名一个标签：


```
$ git tag -s <tagname> -m 'versionname' <commitid>
```
可以推送一个本地标签:

```
git push origin <tagname>
```
可以推送全部未推送过的本地标签:

```
git push origin --tags
```
可以删除一个本地标签:

```
git tag -d <tagname>
```
可以删除一个远程标签:

```
git push origin :refs/tags/<tagname>
```
