---
title: "Dialog"
date: 2017-08-30 13:39
---
[TOC]
## API使用
### 如何设置或取消点击dialog以外区域让dialog消失
```java
Dialog.setCanceledOnTouchOutside(boolean)
```

### 如何将touch时间传递给Window/Dialog下面的View
```java
WindowManager.LayoutParams wmlp = getWindow().getAttributes();
wmlp.flags = wmlp.flags | WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL | WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE;
```