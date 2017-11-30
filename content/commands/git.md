---
title: "git"
date: 2017-09-08 10:08
---
### git远程操作命令
- git clone
- git remote
- git fetch
- git pull
- git push

详见[Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

![](http://image.beekka.com/blog/2014/bg2014061202.jpg)

### git创建远程分支
```java
// 创建study的本地分支
$ git checkout -b study
// 将本地分支study推到远程
$ git push origin study
// 从远程study分支拉取代码
$ git pull origin study
```

### git查看设置
```java
$ git config --list
```
### 设置用户名和邮箱
```java
$ git config --global user.name "李永安"
$ git config --global user.email "hzliyongan@corp.netease.com"
```

### 查看本地项目的upstream已经更新
#### 查看
```java
$ git remote -v
origin	https://github.com/liyonganStudy/PersonalWiki.git (fetch)
origin	https://github.com/liyonganStudy/PersonalWiki.git (push)
```
#### 更新
```java
$ git remote set-url origin https://g.hz.netease.com/cloudmusic-android/Android
```

### 切换分支
#### 查看分支
```java
$ git branch --all
```
#### 切换分支
```java
# 如果本地没有分支
$ git checkout -b <本地分支名> <远程分支名>
# 如 git checkout -b 4.2.1 origin/4.2.1

# 如果本地已有分支
$ git checkout 4.2.1
```
## 保持fork之后的项目和上游同步
### 使用 git remote -v 查看当前的远程仓库地址，输出如下：
```java
origin  git@github.com:ibrother/staticblog.github.io.git (fetch)
origin  git@github.com:ibrother/staticblog.github.io.git (push)
```
接下来运行 git remote add upstream https://github.com/staticblog/staticblog.github.io.git

这条命令就算添加一个别名为 upstream（上游）的地址，指向之前 fork 的原仓库地址。git remote -v 输出如下：

```java
origin  git@github.com:ibrother/staticblog.github.io.git (fetch)
origin  git@github.com:ibrother/staticblog.github.io.git (push)
upstream        https://github.com/staticblog/staticblog.github.io.git (fetch)
upstream        https://github.com/staticblog/staticblog.github.io.git (push)
```
之后运行下面几条命令，就可以保持本地仓库和上游仓库同步了

```
git fetch upstream
git checkout master
git merge upstream/master

git push origin master
```
## 实例操作gitbub网址
### 创建远程库
1. 点击+号下的New repository 创建一个远程库，命名为GitOperate,
1. 版本库类型可以public或者private，程序员都有开源的心，那就public。
1. 还可以勾选Initialize this repository with a README，
1. 接下来可以选择添加.gitignore文件，.gitignore文件有很多类型可以选，
    比如 Android，Android项目下的bin这些文件一般都不需要提交。 
    选择遵循的协议。eg：Apache License 2.0, 这个可以自己去查查每种的意思
1. 点击create，远程版本库就创建完成了，远程版本库的地址为
    https://github.com/FreeSunny/GitOperate.git。
    之后跳转到README.md,该文件主要是对项目的描述。

### 远程库克隆到本地
1. 本地创建一个GitOperate文件夹
1. 远程库地址为https://github.com/FreeSunny/GitOperate.git，
cd进入GitOperate，输入 *git clone https://github.com/FreeSunny/GitOperate.git*
1. 完成后可以在GitOperater文件下的GitOperate文件夹下看到README.md文件（两层文件夹了）
1. 将第二个目录下的所有文件全部复制到上一层目录中，这样就只有第一层目录添加到版本控制中。
    操作命令为(cp -r GitOperate/ .)

### 提交代码
1. git add .// 将提交的代码添加进来，这里指README.md

1. git commit -m “add readme” // 本地提交

1. git push origin master //  同步到远程库

### 本地已经有文件夹
1. cd existing_folder //进入已有文件夹

1.    git init // 初始化

1.    git remote add origin  https://github.com/FreeSunny/GitOperate.git //关联远程库

1.    git add .

1. git commit -m "Initial commit"

1. git push -u origin master

