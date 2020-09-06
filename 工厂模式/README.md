# 工厂模式
工厂模式专门负责实例化有大量公共接口的类。工厂模式可以动态地决定将哪一个类实例化，而不必事先知道每次要实例化哪一个类。客户类和工厂类是分开的。消费者需要某种产品，只需要向工厂发出请求即可。
著名的Spring框架中的IoC就是利用了工厂模式。

IoC 是指在程序开发中，实例的创建不再由调用者管理，而是由 Spring 容器创建。Spring 容器会负责控制程序之间的关系，而不是由程序代码直接控制，因此，控制权由程序代码转移到了 Spring 容器中，控制权发生了反转，这就是 Spring 的 IoC 思想。
简而言之就是，不是由用户来new一个对象，而是交给工厂来完成实例化的过程。

工厂模式包含以下几个形态:
## 简单工厂模式
简单工厂模式(Simple Factory)简单工厂模式的工厂类是根据提供给它的参数，返回的是几个可能产品中一个类的实例，通常情况下，它返回的类都有一个公共的父类和公共的方法。

![avatar](https://img2018.cnblogs.com/blog/1419489/201906/1419489-20190628144601084-563759643.png)

书籍接口
```java
/**
 * 书籍类
 */
public interface Book {
    void make();
}
```

漫画类
```java
/**
 * 漫画类
 */
public class MangaBook implements Book{
    public MangaBook()
    {
        this.make();
    }
    @Override
    public void make()
    {
        System.out.println("生产了一本漫画书");
    }
}
```

小说类
```java
/**
 * 小说类
 */
public class Novel implements Book{
    public void Novel()
    {
        this.make();
    }
    @Override
    public void make()
    {
        System.out.println("生产了一本小说");
    }
}
```

工厂类
```java
/**
 * 生产书的工厂类
 */
public class BookFactory {
    public Book makeBook(String booktype)
    {
        if (booktype.equals("MangaBook")){
            return new MangaBook();
        }
        else if (booktype.equals("Novel")){
            return new Novel();
        }
        return null;
    }
}
```
直接将各种书的类型存在工厂类中，需要创建对象时根据参数创建就行了。
