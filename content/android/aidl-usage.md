---
title: "aidl usage"
date: 2017-09-04 16:06
---

## AIDL作用
AIDL是Android中IPC（Inter-Process Communication）方式中的一种，AIDL是Android Interface definition language的缩写，AIDL可以实现跨进程通信，比如绑定其他进程或者其他app中的service等。

## 简单使用
在A工程中创建aidl文件

```java
// IMyAidlInterface.aidl
package cc.abto.demo;

interface IMyAidlInterface {
	String getName();
}
```
在A工程中定义service

```java
public class MyService extends Service{

    public MyService(){}

    @Override
    public IBinder onBind(Intent intent) {
        return new MyBinder();
    }

    class MyBinder extends IMyAidlInterface.Stub {

        @Override
        public String getName() throws RemoteException {
            return "test";
        }
    }
}
```

在B工程（相当于其他进程）就可以使用A工程中的服务了。

```java
public class MainActivity extends AppCompatActivity {

    private IMyAidlInterface iMyAidlInterface;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bindService(new Intent("cc.abto.server"), new ServiceConnection() {

            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                iMyAidlInterface = IMyAidlInterface.Stub.asInterface(service);
            }

            @Override
            public void onServiceDisconnected(ComponentName name){}
            
        }, BIND_AUTO_CREATE);
    }

    public void onClick(View view) {
        try {
            Toast.makeText(MainActivity.this, iMyAidlInterface.getName(), Toast.LENGTH_SHORT).show();
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
}
```
## 参考文献
[Android中AIDL的使用详解](http://www.jianshu.com/p/d1fac6ccee98)