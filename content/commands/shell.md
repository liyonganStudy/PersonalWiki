---
title: "shell"
date: 2017-08-31 14:56
---

### 修改shell环境变量
#### 1. 修改普通的shell
```
$ vim ~/.bash_profile
$ source ~/.bash_profile
```
#### 2. 修改zsh的配置
```
$ vim ~/.zshrc
$ source ~/.zshrc
```

### cat命令
#### 说明：
主要用于查看文件内容，结合 grep 进行过滤内容信息。
#### 举例：
```
$ cat gitpush.sh | grep add
git add .
git add .
```

### echo/touch 命令
#### 说明：
echo 和 touch 命令就可以方便的写文件
#### 举例：
```
$ touch temp.txt
$ echo "hello" > temp.txt
hello
$ echo "world" >> temp.txt
hello
world
```