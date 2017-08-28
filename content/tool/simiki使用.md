---
title: "simiki使用"
date: 2017-08-28 21:49
collection: simiki
tag: wiki
---

[TOC]

## 基本命令
```
# 生成静态页面到output目录 (需要在站点根目录下执行)
$ simiki g
```

```
# 本地预览模式(开发模式)
$ simiki p
```

```
# 新建页面（包含名字和类别）
$ simiki n/new -t "gettingstarted" -c intro 
$ tree content

content
├── intro
│   └── gettingstarted.md
```

## simiki目录结构
> Simiki使用三层结构模式来管理个人文档目录(content目录)：

>1. 目录(category)：第一层结构，按文件夹存放，用于初步的分类。比如Linux、Python、工具等等。
1. 集合(collection)：第二层结构，在页面(page)的meta中定义（关键字: collection），用于进一步的分类，效果上和子目录一样。一个页面只能属于一个集合。
1. 标签(tag)：第三层结构，在页面(page)的meta中定义（关键字：tag）。一个页面可以有多个tags。

### 集合
>在同一个目录下，配置了相同的集合(collection)后：

>* 同一个目录下，未配置集合(collection)的页面(page)和集合是同一级结构
* 同一个目录下，配置集合(collection)的页面(page)处于集合中更深一级的结构中

#### 如何配置集合
只需要在头部信息中增加```collection```属性

```
$ cat content/tool/{git,svn}.md
---
title: "Git"
date: 2016-01-01 00:00
collection: Version Control
---

---
title: "SVN"
date: 2016-01-02 00:00
collection: Version Control
---
```

### 标签
>标签使多篇Wiki之间互相有关联性。
如下例子，两篇Wiki，都是和Vim相关的：

```
==> content/book/practical-vim.md <==
---
title: "Practical Vim"
date: 2014-11-13 22:11
tag: vim  # 以逗号分隔的字符串方式配置tag
---

==> content/tool/vim.md <==
---
title: "Vim"
date: 2013-08-17 07:32
tag:  # 以列表的方式配置tag
  - vim
---
```
## simiki文档
[simiki文档地址](http://simiki.org/zh-docs/)