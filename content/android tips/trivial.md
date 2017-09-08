---
title: "trivial"
date: 2017-09-08 10:05
---

## 如何log记录StackTrace
```java
    public static void logStackTrace(String title) {
        log("===============" + title + "\n");
        StackTraceElement elements[] = Thread.currentThread().getStackTrace();
        for (StackTraceElement item : elements) {
            if (item.isNativeMethod()) {
                continue;
            }
            String cn = item.getClassName();
            String mn = item.getMethodName();
            String filename = item.getFileName();
            int line = item.getLineNumber();
            log(cn + "." + mn + "(" + filename + ":" + line + ")" + "\n");
        }
    }
```