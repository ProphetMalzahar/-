# 单例模式


参考了菜鸟教程的信息

[链接](https://www.baidu.com/linkurl=zyfC5ceSKYupGVjuqodcX58kY262QMPHJhOsJEX6SlSR1XBa5d1oKP518C83GMQFxLLzhFX5GOl_iPudWPxfWq75FrMAmAagCCb2k4dvjra&wd=&eqid=faff8c73000436ec000000055f52f030)

单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。单例模式的核心之一是构造方法私有化。
单例模式一般有5种实现方式：饿汉式、懒汉式、双重检查、静态内部类、枚举。
注意：
1、单例类只能有一个实例。
2、单例类必须自己创建自己的唯一实例。
3、单例类必须给所有其他对象提供这一实例。

## 懒汉式
懒汉式分为线程不安全和线程安全两种
### 线程不安全
实现了懒加载，但没有加锁，线程不安全，所以严格意义上不能算是单例模式。
```java
/**
 * 懒汉式，线程不安全
 */
public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static Singleton getInstance(){
        if (instance==null)
        {
            instance=new Singleton();
        }
        return instance;
    }
}
```
### 线程安全
其实就是在线程不安全的基础上，给getInstance方法加了锁。实现了懒加载，因为加了锁，所以线程安全。
```java
/**
 * 懒汉式，线程安全
 */
public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static synchronized Singleton getInstance(){
        if (instance==null)
        {
            instance=new Singleton();
        }
        return instance;
    }
}
```

## 饿汉式
instance 在类装载时就实例化，并不是懒加载。它基于classloader机制避免了多线程的同步问题，是线程安全的。
```java
/**
 * 饿汉式，线程安全
 */
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton (){}  
    public static Singleton getInstance() {  
    return instance;  
    }  
}
```

## 双检锁
懒加载，采用双锁机制，安全且在多线程情况下能保持高性能。getInstance() 的性能对应用程序很关键。
```java
/**
 * 双检锁，线程安全
 */
public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
    if (singleton == null) {  
        synchronized (Singleton.class) {  
        if (singleton == null) {  
            singleton = new Singleton();  
        }  
        }  
    }  
    return singleton;  
    }  
}
```

## 静态内部类
静态内部类的特点是：当主类加载时，静态内部类不会被加载；只有使用到该静态内部类时，才会加载，所以实现了懒加载。
且在加载时，由于JVM类加载保护机制，会是一个线程安全的过程；且静态内部类只会被加载一次。
```java
/**
 * 静态内部类，线程安全
 * PS：这是网易的一道笔试题
 */
public class Demo {
    private Demo() {}
    private static class Singleton {
        private static final Demo INSTANCE = new Demo();
    }
    public static Demo getInstance() {
        return Singleton.INSTANCE;
    }
}
```
## 枚举
线程安全，未实现懒加载。
```java
public enum Singleton {  
    INSTANCE;  
    public void whateverMethod() {  
    }  
}
```
> 一般情况下，懒汉式（包含线程安全和线程不安全两种方式）都比较少用；饿汉式和双检锁都可以使用，可根据具体情况自主选择；在要明确实现 lazy loading 效果时，可以考虑静态内部类的实现方式；若涉及到反序列化创建对象时，大家也可以尝试使用枚举方式。
