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