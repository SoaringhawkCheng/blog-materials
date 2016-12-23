---
title: 在Github上部署自己的简历
date: 2016-12-23 15:15:18
categories: 编程
tags: Git
---
当对幸福的憧憬过于急切，那痛苦就在人的心灵深处升起。
						————阿贝尔·加缪

<!--more-->

## Github部署简历
### 第1步：创建SSH Key
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key:

```
$ ssh-keygen -t rsa -C "2856043766@qq.com"
```

然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

### 第2步：登陆GitHub
打开“Account settings”，“SSH Keys”页面。然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。Mac无法之间打开`id_rsa.pub`,可以在终端输入：

```
$ more id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDI1d9ac4WEhAeO8YqaBF1nVEtnZS2lDUOxXmz/4giJtAh1BfFkUvMTN6oPAZ1dqyzMXM+w4vzyyujQaHyu2tApJTt0/oJb+Fz3z5oUOxvDdfi4m9X255ELAlRXaxQ4EeCoqSLOmaTibePUOgiWNE+SGVP87nB3zRzN9uiVbfq4M42EL1Pmd41s666pm3bk4bYy++ANiuvlsCK3yw8J/HUcg83ABZGdKURkI6B73U1lhsYRvqc+tcCrOp07JbqWSfbeJq09YhsB38Tmj+ZC9+AwGhZLdAwiyQR30RsFF9s4aAOzPftBbHbI6ZcY9EfmxgFbunw/pNOR+a1XYrMG0s7x 2856043766@qq.com
```
把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。
由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

### 第3步：同步Git仓库
在本地创建了一个Git仓库后，又在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。

目前，在GitHub上新建一个cv仓库，由于仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：

```
$ git remote add origin git@github.com:SoaringhawkCheng/cv.git
```

第一次推送：

```
$ git push -u origin master
```
以后再推送：

```
$ git push origin master
```