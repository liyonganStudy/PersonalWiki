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

## android:process
* 这个属性可以让组件运行在不同的进程中，如果process name以：开头，表示这个新进程是程序的私有进程。
* 设置了 android:process 属性将组件运行到另一个进程，相当于另一个应用程序，所以在另一个线程中也将新建一个 Application 的实例。因此，每新建一个进程 Application 的 onCreate 都将被调用一次。 

获取当前进程名称：
### 方法1
```java
public static String getProcessName(Context cxt) {  
	// 需要try catch
	int pid = android.os.Process.myPid();
    ActivityManager am = (ActivityManager) cxt.getSystemService(Context.ACTIVITY_SERVICE);  
    List<RunningAppProcessInfo> runningApps = am.getRunningAppProcesses();  
    if (runningApps == null) {  
        return null;  
    }  
    for (RunningAppProcessInfo procInfo : runningApps) {  
        if (procInfo.pid == pid) {  
            return procInfo.processName;  
        }  
    }  
    return null;  
}  
```
### 方法2
```java
public static String getProcessName() {
  try {
    File file = new File("/proc/" + android.os.Process.myPid() + "/" + "cmdline");
    BufferedReader mBufferedReader = new BufferedReader(new FileReader(file));
    String processName = mBufferedReader.readLine().trim();
    mBufferedReader.close();
    return processName;
  } catch (Exception e) {
    e.printStackTrace();
    return null;
  }
}
```