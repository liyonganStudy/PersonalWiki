---
title: "classloader"
date: 2017-08-31 15:46
---
[TOC]
## 什么是ClassLoader
Java有个概念叫做类加载器（ClassLoader），它的作用就是`动态的装载Class文件`。借助ClassLoader这个类我们可以装载任何我们想要的Class文件，只需提供路径即可。
#### 为什么需要classLoader
使用import的话得类文件必须在本地且编译的时候必须有这个类文件，否则报错。很多场景需要在没有本地类文件时使用这个类，需要动态编译。
#### 什么是相同的类
>同一个Class = 相同的 ClassName + PackageName + ClassLoader

### 都有哪些classloader
#### BootClassLoader
系统启动的时候创建的
#### PathClassLoader
* 应用启动时创建的，用于加载“/data/app/com.netease.cloudmusic-1/base.apk”里面的类
* PathClassLoader只能加载系统中已经安装过的apk

#### DexClassLoader
* Android会重新打包对Class文件内部的各种函数表、变量表等进行优化，最终产生了dex文件。dex文件是一种经过android打包工具优化后的Class文件，因此加载这样特殊的Class文件就需要特殊的类装载器，所以android中提供了DexClassLoader类。
* DexClassLoader可以加载jar/apk/dex，可以从SD卡中加载未安装的apk

## classloader双亲代理模型
### classloader和parent
创建一个ClassLoader实例的时候，需要使用一个现有的ClassLoader实例作为新创建的实例的Parent。这样一来，一个Android应用，甚至整个Android系统里所有的ClassLoader实例都会被一棵树关联起来，这也是ClassLoader的 双亲代理模型（Parent-Delegation Model）的特点。

```java
    /*
     * constructor for the BootClassLoader which needs parent to be null.
     */
    ClassLoader(ClassLoader parentLoader, boolean nullAllowed) {
        if (parentLoader == null && !nullAllowed) {
            throw new NullPointerException("parentLoader == null && !nullAllowed");
        }
        parent = parentLoader;
    }
```
### 加载class时
从源码中我们也可以看出，loadClass方法在加载一个类的实例的时候，

1. 会先查询当前ClassLoader实例是否加载过此类，有就返回；
1. 如果没有。查询Parent是否已经加载过此类，如果已经加载过，就直接返回Parent加载的类；
1. 如果继承路线上的ClassLoader都没有加载，才由Child执行类的加载工作；

```java
    protected Class<?> loadClass(String className, boolean resolve) throws ClassNotFoundException {
        Class<?> clazz = findLoadedClass(className);

        if (clazz == null) {
            ClassNotFoundException suppressed = null;
            try {
                clazz = parent.loadClass(className, false);
            } catch (ClassNotFoundException e) {
                suppressed = e;
            }

            if (clazz == null) {
                try {
                    clazz = findClass(className);
                } catch (ClassNotFoundException e) {
                    e.addSuppressed(suppressed);
                    throw e;
                }
            }
        }

        return clazz;
    }
```
## 使用BaseDexClassLoader加载dex中的类
### DexClassLoader 和 PathClassLoader
DexClassLoader可以指定自己的optimizedDirectory，所以它可以加载外部的dex，因为这个dex会被复制到内部路径的optimizedDirectory；而PathClassLoader没有optimizedDirectory，所以它只能加载内部的dex，这些大都是存在系统中已经安装过的apk里面的。

```java
// DexClassLoader.java
public class DexClassLoader extends BaseDexClassLoader {
    public DexClassLoader(String dexPath, String optimizedDirectory,
            String libraryPath, ClassLoader parent) {
        super(dexPath, new File(optimizedDirectory), libraryPath, parent);
    }
}

// PathClassLoader.java
public class PathClassLoader extends BaseDexClassLoader {
    public PathClassLoader(String dexPath, ClassLoader parent) {
        super(dexPath, null, null, parent);
    }

    public PathClassLoader(String dexPath, String libraryPath,
            ClassLoader parent) {
        super(dexPath, null, libraryPath, parent);
    }
}

public BaseDexClassLoader(String dexPath, File optimizedDirectory,
           String libraryPath, ClassLoader parent) {
        super(parent);
        this.pathList = new DexPathList(this, dexPath, libraryPath, optimizedDirectory);
    }
```

### optimizedDirectory的作用
optimizedDirectory必须是一个内部存储路径，还记得我们之前说过的，无论哪种动态加载，加载的可执行文件一定要存放在内部存储。

```java
    public DexPathList(ClassLoader definingContext, String dexPath,
            String libraryPath, File optimizedDirectory) {
        ……
        this.dexElements = makeDexElements(splitDexPath(dexPath), optimizedDirectory);
    }

    private static Element[] makeDexElements(ArrayList<File> files,
            File optimizedDirectory) {
        ArrayList<Element> elements = new ArrayList<Element>();
        for (File file : files) {
            ZipFile zip = null;
            DexFile dex = null;
            String name = file.getName();
            if (name.endsWith(DEX_SUFFIX)) {
                dex = loadDexFile(file, optimizedDirectory);
            } else if (name.endsWith(APK_SUFFIX) || name.endsWith(JAR_SUFFIX)
                    || name.endsWith(ZIP_SUFFIX)) {
                zip = new ZipFile(file);
            }
            ……
            if ((zip != null) || (dex != null)) {
                elements.add(new Element(file, zip, dex));
            }
        }
        return elements.toArray(new Element[elements.size()]);
    }
    
        private static DexFile loadDexFile(File file, File optimizedDirectory)
            throws IOException {
        if (optimizedDirectory == null) {
            return new DexFile(file);
        } else {
            String optimizedPath = optimizedPathFor(file, optimizedDirectory);
            return DexFile.loadDex(file.getPath(), optimizedPath, 0);
        }
    }

```
### 加载类的过程
上面创建了DexFile，classloader最终通过DexFile的defineClass加载类。
classload-->DexPathList--> dexElements--> DexFile

```java
  public Class<?> loadClass(String className) throws ClassNotFoundException {
        return loadClass(className, false);
    }

    protected Class<?> loadClass(String className, boolean resolve) throws ClassNotFoundException {
        Class<?> clazz = findLoadedClass(className);
        if (clazz == null) {
            ClassNotFoundException suppressed = null;
            try {
                clazz = parent.loadClass(className, false);
            } catch (ClassNotFoundException e) {
                suppressed = e;
            }

            if (clazz == null) {
                try {
                    clazz = findClass(className);
                } catch (ClassNotFoundException e) {
                    e.addSuppressed(suppressed);
                    throw e;
                }
            }
        }
        return clazz;
    }

	@Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        Class clazz = pathList.findClass(name);
        if (clazz == null) {
            throw new ClassNotFoundException(name);
        }
        return clazz;
    }
    
    public Class findClass(String name) {
        for (Element element : dexElements) {
            DexFile dex = element.dexFile;
            if (dex != null) {
                Class clazz = dex.loadClassBinaryName(name, definingContext);
                if (clazz != null) {
                    return clazz;
                }
            }
        }
        return null;
    }
    
    public Class loadClassBinaryName(String name, ClassLoader loader) {
        return defineClass(name, loader, mCookie);
    }
   	private native static Class defineClass(String name, ClassLoader loader, int cookie);
```

## 参考资料
[Android动态加载基础 ClassLoader工作机制](https://segmentfault.com/a/1190000004062880)