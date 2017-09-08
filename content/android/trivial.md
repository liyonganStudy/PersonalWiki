---
title: "trivial"
date: 2017-09-07 15:33
---

### ComponentName
#### 说明：
* Intent的Component属性要接受一个ComponentName对象
* 实例化一个ComponentName需要两个参数，第一个参数是要启动应用的包名称，第二个参数是你要启动的Activity或者Service的全称（包名+类名）
* 如果你要的启动的其他应用的Activity不是该应用的入口Activity，那么在清单文件中，该Activity节点一定要加上android:exported="true"，表示允许其他应用打开，对于所有的Service也是一样

```java
ComponentName comp = new ComponentName("com.lovo.activity",  
                        "com.lovo.activity.SecondActivity");  
Intent intent = new Intent();  
// 为Intent设置Component属性  
intent.setComponent(comp);  
startActivity(intent);  
```