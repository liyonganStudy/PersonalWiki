---
title: "adb"
date: 2017-08-31 17:53
---

## adb shell screencap
说明：截屏操作

用法：

    adb shell screencap –p 截图文件路径

案例：

    adb shell screencap –p /sdcard/tmp.png

这个命令对于测试人员非常有用，有时候想快速截取手机屏幕，速度打开，我们就可以利用这个命令写一个简单的脚本文件，内容如下：

```java
adb shell screencap -p /sdcard/tmp.png
adb pull /sdcard/tmp.png ~/Desktop
open ~/Desktop/tmp.png
```