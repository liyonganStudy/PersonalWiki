---
title: "git"
date: 2017-09-08 10:08
---

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