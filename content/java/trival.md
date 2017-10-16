---
title: "trival"
date: 2017-10-16 15:47
---

### Math小数值舍入操作方法：ceil()、floor()、round()
Math对象中有3个方法用于处理小数值的舍入操作，它们是：Math.ceil()、Math.floor()、Math.round()。

Math.ceil()：向上舍入为最接近的整数。

Math.floor()：向下舍入为最接近的整数。

Math.round()：标准的四舍五入。

```java
alert(Math.ceil(3.2));   // 4
alert(Math.ceil(3.5));   // 4
alert(Math.ceil(3.6));   // 4

alert(Math.floor(3.2));  // 3
alert(Math.floor(3.5));  // 3
alert(Math.floor(3.6));  // 3

alert(Math.round(3.2));  // 3
alert(Math.round(3.5));  // 4
alert(Math.round(3.6));  // 4
```