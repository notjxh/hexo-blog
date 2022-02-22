---
title: git教程
date: 2022-02-22 11:14:21
tags: git
description: git的使用教程
categories: 开发工具
---

## Git简介
### windows安装git
从Git官网[下载安装程序](https://git-scm.com/downloads),默认安装后启动git bash，设置如下参数  
>     git config --global user.name "jx*"
>     git config --global user.email "578***@qq.com"

https://www.cnblogs.com/specter45/p/github.html

---

### 创建版本库 Repository
>     cd f: //进入f盘
>     mkdir git-demo //创建git-demo目录 cd git-demo
>     git init //初始化一个Git仓库：将git -demo用git管理
>     ls -ah //查看隐藏的.git文件
>     手动创建一个readme.txt文件
>     git add readme.txt  //添加到git仓库
>     git commit -m "提交readme.txt" //提交到git仓库


---

## 时光机穿梭

```
$ git status //查看git仓库当前状态
On branch master
nothing to commit, working tree clean
```


>  `git status`查看git当前状态 

---

手动修改readme.txt 

```
$ git diff
diff --git a/readme.txt b/readme.txt
index 013b5bc..d8036c1 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a distributed version control system.
+Git is a version control system.
 Git is free software.
\ No newline at end of file

```
>  `git diff`查看文件变动

---

### 版本回退
修改readme.txt的内容如下
```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```
> git add readme.txt git commit -m "修改“ 再次提交

```
$ git log
commit 4b52a930abea992268bb62b0ad5c786acf89b634
Author: jxh <578995394@qq.com>
Date:   Wed Feb 28 10:08:31 2018 +0800

    append GPL

commit 170a0367923226396094e983fa6966bfb2029d12
Author: jxh <578995394@qq.com>
Date:   Wed Feb 28 10:00:31 2018 +0800

    修改readme.txtt

commit a151e5972f50310dc8f3023a28e58161c6f61c4a
Author: jxh <578995394@qq.com>
Date:   Wed Feb 28 09:41:04 2018 +0800

    提交readme文件


```

> `git log`查看提交历史
---

```
$ git log --pretty=oneline
4b52a930abea992268bb62b0ad5c786acf89b634 append GPL
170a0367923226396094e983fa6966bfb2029d12 修改readme.txt
a151e5972f50310dc8f3023a28e58161c6f61c4a 提交readme文件

```
> `git log --pretty=oneline`简化log显示结果
---


commit 后的commit id表示提交版本号，git中使用`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`..`HEAD~100`表示上100个版本  

```
回退到上一个版本 --hard参数
$ git reset --hard HEAD^
HEAD is now at 170a036 修改readme.txt
```
`cat readme.txt` 查看readme.tex内容  
`git log` 看不到append GPL的那次提交了  
如果想恢复到GPL，则可通过append GPL的commit id找回  
`git reset --hard 4b32a`  


> `git reset --hard`来回退到某个版本号  
---


```
$ git reflog
4b52a93 HEAD@{0}: reset: moving to 4b52a
170a036 HEAD@{1}: reset: moving to HEAD^
4b52a93 HEAD@{2}: commit: append GPL
170a036 HEAD@{3}: commit: 修改readme.txtt
a151e59 HEAD@{4}: commit (initial): 提交readme文件

```

> `git reflog`可以查看git的每次操作，用来找对应的commit id
---

### 工作区和暂存区 见图
`工作区`是指我们实际能看到和操作的文件目录  
`暂存区`是git版本库的一部分  
git add 实际是将工作区的内容更新到暂存区  
git commit 是将暂存区中的内容更新到分支  

### 管理修改
git 不是追踪文件，而是追踪文件的修改，因此每次修改之后需要`git add`修改， 再`git commit`到版本库

### 撤销修改
1. 只在工作区修改了文件，没有add到暂存区时  
如在readme.txt中添加一行stupid things

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

```
> 可以通过`git checkout -- readme.txt`撤销工作区中的变动  
---

3. 在工作区修改了并通过`git add`添加到暂存区

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt

```

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
```

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

```
> 通过以上三步撤销暂存区中的改动，恢复到情况1
`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。
---

3. 如果commit 到版本库了，但没有提交到远程库，可以通过版本回退撤销

### 撤销删除
> `rm test-delete.txt` 删除工作区文件  
通过`git checkout -- test-delete.txt` 撤销删除  
`git add test-delete.txt` 并提交到仓库  
`git rm test-delete.txt` 相当于上面两步  
通过版本回退`git reset --hard HEAD^`  撤销删除，但会丢失最近一次提交之后修改的内容
---

## 远程仓库
    github提供了git仓库的托管服务
1. 创建SSH key 

```
ssh-keygen -t rsa -C "578995394@qq.com"
```
在C:\Users\jxh\.ssh中生成了三个文件`id_rsa`私钥，`id_rsa.pub`公钥，`known_hosts`，将公钥拷贝到github账户中的settings-SSH key 中

#### 添加远程库
现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。
1. 在github新建一个repository，作为远程git仓库 
2. 将本地git仓库关联到github仓库

```
$ git remote add origin git@github.com:XiaoHuJiang/git-demo.git

```

3. 第一次推送本地仓库内容到远程库

```
$ git push -u origin master
Warning: Permanently added the RSA host key for IP address '52.74.223.119' to the list of known hosts.
Counting objects: 12, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (9/9), done.
Writing objects: 100% (12/12), 1.11 KiB | 0 bytes/s, done.
Total 12 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
Branch master set up to track remote branch master from origin.
To github.com:XiaoHuJiang/git-demo.git
 * [new branch]      master -> master

```
> `$ git push -u origin master`
`-u`第一次推送时加上可以将本地仓库与远程库关联
---

4. 之后推送
`git push origun master`

### 从远程仓库克隆
上一节说了先创建本地仓库，再关联远程仓库，
这节先创建远程仓库，再clone到本地

```
$ git clone git@github.com:XiaoHuJiang/git-demo.git
Cloning into 'git-demo'...
Warning: Permanently added the RSA host key for IP address '13.250.177.223' to the list of known hosts.
remote: Counting objects: 12, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 1), reused 12 (delta 1), pack-reused 0
Receiving objects: 100% (12/12), done.
Resolving deltas: 100% (1/1), done.
```
> `git clone git@github.com:XiaoHuJiang/git-demo.git` 
---


## 分支管理
分支概念：比如你想开发一个新功能，可以新建一个分支，在这个分支上开发，等开发完了再一起合并到master分支上，这样不会影响到master的正常使用
#### 创建和合并分支
    流程图可看原教程，总结如下
`git branch` 查看分支  
`git branch dev` 创建分支  
`git checkout dev` 切换分支  
`git checkout -b dev` 创建并切换到分支dev  
`git merge dev` 合并dev到master（mastee分支下）  
`git branch -d dev` 删除dev(master分支下)  
如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 解决冲突
没实现冲突,查看分支合并：  
` git log --graph --pretty=oneline --abbrev-commit`
### 分支管理策略
master分支稳定用来发布版本，在dev上开发  
`git merge --no-ff -m "merge with no-ff" dev`   
不使用fast-forward 方式合并，可看到合并历史


