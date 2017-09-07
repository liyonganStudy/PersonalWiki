---
title: "error case"
date: 2017-09-07 14:37
---

### 1. Release版本会proguard，将没有用到的方法直接删除，也会简写方法名等。需要测试版可以，release版本不行的原因都是proguard导致的。

### 2. 界面复用时，可以同时点击两处复用入口可能会出现NPE异常，原因是是复用时会将一些数据置位null，这时如果没有将clickListener置为null，会触发clickListener中的回调，此时由于这些数据被置为null会产生NPE。