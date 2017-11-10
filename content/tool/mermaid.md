---
title: "mermaid"
date: 2017-11-10 14:24
---

### 常用语法
#### 箭头
```java
sequenceDiagram
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
```
#### 使用别名
```java
sequenceDiagram
    participant A as Alice
    participant J as John
    A->>J: Hello John, how are you?
    J->>A: Great!
```
#### 使用标注
```java
sequenceDiagram
    participant John
    Note right of John: Text in note

sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```
### 参考链接
[官方文档](https://mermaidjs.github.io/sequenceDiagram.html)