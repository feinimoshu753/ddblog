---
title: git常用内容整理
date: 2017-09-28 06:52:38
tags: 前端
---
git算是最常用的代码管理工具了，虽然平时经常用到，但是很少使用命令行进行操作，而且部分命令也不是很熟悉。so，进行git常用的内容进行整理。

<!-- more -->

#### 一、git初始化
```
$ mkdir git-test
$ cd git-test
$ git init
```
#### 二、加入暂存区
首先创建一个test.txt文件加入git所在目录（或子目录），同时编辑test.txt增加部分文字内容。

```
$ git add test.txt
```
#### 三、提交暂存区内容到本地

```
$ git commit -m "create test.txt"
```
#### 四、查看当前git状态

```
$ git status
```
#### 五、查看文件修改内容

```
$ git diff test.txt
$ git diff HEAD -- test.txt（查看工作区和版本库最新文件的区别）
```
#### 六、查看git记录

```
$ git log（当前git记录）
$ git log --pretty=oneline（仅显示一行SHA1值）
$ git reflog（显示所有git操作命令）
$ git log --graph --pretty=oneline --abbrev-commit（查看分支合并情况）
```
#### 七、版本回退

```
$ git reset --hard HEAD^ 回退到上一次版本
$ git reset --hard HEAD^^^^^^^^^^ 或 git reset --hard HEAD~10回退到上十个版本
$ git reset --hard f950187848e2b3bfed801cd08e707c4471292fa0（某次提交的commit id）回退某个提交版本
```
#### 八、撤销修改

```
$ git checkout -- test.txt（撤销工作区内容）
$ git reset HEAD test.txt（撤销暂存区内容到工作区）
```
#### 九、文件删除

```
$ git rm test.txt
```

#### 十、同步本地内容到远程

```
$ git remote add origin git@xxxxx （本地关联远程库）
$ git push -u origin master（本地所有内容提交到远程库）
$ git push origin master（提交最新修改到远程库）
```

#### 十一、远程库克隆到本地

```
$ git clone git@xxxxx
```
#### 十二、查看远程库信息

```
$ git remote -v
```

#### 十三、从远程库同步内容

```
$ git pull
```

#### 十四、本地创建和远程对应分支

```
$ git checkout -b branch-name origin/branch-name
```
#### 十五、本地分支和远程分支建立连接

```
$ git branch --set-upstream branch-name origin/branch-name
```

#### 十六、创建分支

```
$ git branch dev
```

#### 十七、切换分支

```
$ git checkout dev（切换分支到dev）
$ git checkout -b dev（创建dev分支并切换到dev）
```
#### 十八、查看分支

```
$ git branch
```
#### 十九、合并分支

```
$ git merge dev
$ git merge --no-ff -m "merge with no-ff" dev（不适用fast-forward合并分支）
```
#### 二十、删除分支

```
$ git branch -d dev
$ git branch -D dev（强制删除分支）
```
#### 二十一、存储当前工作现场

```
$ git stash
```
#### 二十二、存储工作现场列表

```
$ git stash list
```
#### 二十三、恢复工作现场

```
$ git stash apply（恢复同时保留stash）
$ git stash apply stash@{0}（恢复到某个stash）
$ git stash pop（恢复同时删除stash）
```
#### 二十四、创建标签

```
$ git tag v20170928
$ git tag v20170928 f950187848e2b3bfed801cd08e707c4471292fa0（为某次提交打tag）
git tag -a v20170928 -m "version tag info" f950187848e2b3bfed801cd08e707c4471292fa0（创建带说明的标签）
```
#### 二十五、查看所有标签

```
$ git tag
```

#### 二十六、查看某个标签信息

```
$ git show tag-name
```
#### 二十七、删除某个标签

```
$ git tag -d v20170928
git push origin :refs/tags/v20170928（同步删除远程库标签）
```
#### 二十八、同步本地标签到远程库

```
$ git push origin v20170928
$ git push origin --tags（同步所有标签）
```

#### 参考文章
[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

over...