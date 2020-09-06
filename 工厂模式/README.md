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
    public Novel()
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
演示:
```java
public class Main {
    public static void main(String[] args) {
        BookFactory factory=new BookFactory();
        Book book=factory.makeBook("MangaBook");
        Book novel=factory.makeBook("Novel");
    }
}
```
输出:

生产了一本漫画书

生产了一本小说

可以看出，简单工厂模式中直接将各种书的类型存在工厂类中，需要创建对象时根据参数创建就行了。

## 工厂方法模式
工厂方法模式(Factory Method)是类的创建模式，其用意是定义一个用于创建产品对象的工厂的接口，而将实际创建工作推迟到工厂接口的子类中。它属于简单工厂模式的进一步抽象和推广。
多态的使用使得工厂方法模式保持了简单工厂模式的优点，而且克服了它的缺点。

![avatar](https://img2018.cnblogs.com/blog/1419489/201906/1419489-20190628154133368-906051111.png)
产品的几个类与上同

抽象工厂类
```java
/**
 * 生产不同书的抽象工厂类
 */
public interface AbstractFactory {
    public Book makeBook();
}
```
生产漫画的工厂1
```java
/**
 * 生产漫画的工厂
 */
public class MangaFactory implements AbstractFactory{
    @Override
    public Book makeBook() {
        return new MangaBook();
    }
}
```
生产小说的工厂2
```java
/**
 * 生产小说的工厂
 */
public class NovelFactory implements AbstractFactory{
    @Override
    public Book makeBook() {
        return new Novel();
    }
}
```
演示：
```java
public class Main {
    public static void main(String[] args) {
        AbstractFactory mangafactory=new MangaFactory();
        AbstractFactory novelfactory=new NovelFactory();
        mangafactory.makeBook();
        novelfactory.makeBook();
    }
}
```
输出与上同

## 抽象工厂模式
抽象工厂模式(Abstract Factory)是所有形态的工厂模式中最为抽象也最具一般性的一种形态。此模式为创建一组相关或相互依赖的对象提供一个接口，而且无需指定他们的具体类。抽象工厂模式通过在AbstarctFactory中增加创建产品的接口，并在具体子工厂中实现新加产品的创建。
![img](https://img2018.cnblogs.com/blog/1419489/201906/1419489-20190628170705865-1781414242.png)

新增动画接口，和两个动画类
```java
/**
 * 动画接口
 */
public interface Anime {
    void make();
}
```

```java
/**
 * 漫画改编动画
 */
public class MangaAnime implements Anime{
    public MangaAnime(){
        this.make();
    }
    @Override
    public void make() {
        System.out.println("一部漫画改编动画产品");
    }
}
```

```java
/**
 * 小说改编动画
 */
public class NovelAnime implements Anime{
    public NovelAnime() {
        this.make();
    }
    @Override
    public void make() {
        System.out.println("一部小说改编动画产品");
    }
}
```
在AbstractFactory类中添加makeAnime()方法
```java
/**
 * 生产不同书的抽象工厂类
 */
public interface AbstractFactory {
    public Book makeBook();
    public Anime makeAnime();
}
```
在具体的工厂中添加实现makeAnime()方法
```java
/**
 * 生产漫画相关产品的工厂
 */
public class MangaFactory implements AbstractFactory{
    @Override
    public Book makeBook() {
        return new MangaBook();
    }

    @Override
    public Anime makeAnime() {
        return new MangaAnime();
    }
}
```
```java
/**
 * 生产小说相关产品的工厂
 */
public class NovelFactory implements AbstractFactory{
    @Override
    public Book makeBook() {
        return new Novel();
    }

    @Override
    public Anime makeAnime() {
        return new NovelAnime();
    }
}
```
演示
```java
public class Main {
    public static void main(String[] args) {
        AbstractFactory mangafactory=new MangaFactory();
        AbstractFactory novelfactory=new NovelFactory();
        mangafactory.makeBook();
        novelfactory.makeBook();
        mangafactory.makeAnime();
        novelfactory.makeAnime();
    }
}
```
输出：

生产了一本漫画书

生产了一本小说

一部漫画改编动画产品

一部小说改编动画产品

总结：三种工厂模式有各自的特点和适用场景，比如抽象工厂模式就适用于系统的产品有多于一个的产品族，而系统只消费其中某一族的产品的情况。要根据具体情况使用。而且已成型的工厂通常比较复杂，增加产品就需要修改具体的工厂类。

部分引用自博客https://www.cnblogs.com/yssjun/p/11102162.html
