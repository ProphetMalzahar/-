# 代理模式
**代理模式**（Proxy Pattern）是程序设计中的一种设计模式。
所谓的代理者是指一个类别可以作为其它东西的接口。代理者可以作任何东西的接口：网上连接、存储器中的大对象、文件或其它昂贵或无法复制的资源。
代理模式的定义：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。
代理模式分为静态代理和动态代理

## 静态代理
静态代理是由程序员创建或特定工具自动生成源代码，也就是在编译时就已经将接口，被代理类，代理类等确定下来。在程序运行之前，代理类的.class文件就已经生成。

我们可以举一个简单的例子，小糖，是一个老二次元，他想要买BD，但是又不方便去日本，而且自己去也比较麻烦，这时候他就可以去找人代买，比如淘宝的中介。

创建购买类的接口
```java
/**
 * 购买BD的行为
 */
public interface BuyBD {
    void buybd();
}
```
实现这一接口
```java
/**
 * 实现购买的行为
 */
public class BuyBDImpl implements BuyBD{
    @Override
    public void buybd() {
        System.out.println("购买BD");
    }
}
```
实现代理的类
```java
/**
 * 代理类
 */
public class BuyBDProxy implements BuyBD{
    BuyBD buyBD;

    public BuyBDProxy(BuyBD buyBD) {
        this.buyBD = buyBD;
    }

    @Override
    public void buybd() {
        System.out.println("中介买书中");
        buyBD.buybd();
        System.out.println("买书成功");
    }
}
```
最后测试一下
```java

public class Main {
    public static void main(String[] args) {
        BuyBD buyBD=new BuyBDImpl();
        buyBD.buybd();
        BuyBDProxy buyBDProxy=new BuyBDProxy(buyBD);
        buyBDProxy.buybd();
    }
}
```
静态代理总结：
我们直接创建一个BuyBD对象，把它传给代理类，而购买BD的这个行为，就不是由BuyBD对象，而是由代理类来执行。我们可以在切点的前后加入一些内容，利用代理模式就很容易实现。

优点：可以做到在符合开闭原则的情况下对目标对象进行功能扩展。

缺点：我们得为每一个服务都得创建代理类，工作量太大，不易管理。同时接口一旦发生改变，代理类也得相应修改。

## 动态代理
代理类在程序运行时创建的代理方式被称为动态代理。 在静态代理的中，代理类是自己定义好的，在程序运行之前就已经编译完成。然而动态代理，代理类并不是在Java代码中定义的，而是在运行时根据我们在Java代码中的“指示”动态生成的。
相比于静态代理， 动态代理的优势在于可以很方便的对代理类的函数进行统一的处理，而不用修改每个代理类中的方法。
比较常见的例子就是Spring的AOP了，Spring AOP通过动态代理对目标类进行增强。

java的java.lang.reflect包下提供了一个Proxy类和一个InvocationHandler接口，通过这个类和这个接口可以生成JDK动态代理类和动态代理对象。
动态代理需要编写一个动态处理器
```java
/**
 * 动态处理器，实现InvocationHandler接口
 */
public class BuyBDProxyHandler implements InvocationHandler {
    Object object;

    public BuyBDProxyHandler(Object object) {
        this.object = object;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("中介购买中");
        Object obj=method.invoke(object,args);
        System.out.println("买书成功");
        return obj;
    }
}
```
修改测试类，这里面需要应用到Java的反射机制。
```java
public class Main {
    public static void main(String[] args) {
        BuyBD buyBD=new BuyBDImpl();
        BuyBD buyBDProxy=(BuyBD) Proxy.newProxyInstance(BuyBD.class.getClassLoader(),new Class[]{BuyBD.class},new BuyBDProxyHandler(buyBD));
        buyBDProxy.buybd();
    }
}
```
我们创建一个被代理的buyBD对象，将这个对象传到了BuyBDProxyHandler中，最后执行的是动态处理器中的invoke方法。所以我们也就能看到和静态代理中一样的输出结果。

动态代理的优势在于可以很方便的对代理类的函数进行统一的处理，而不用修改每个代理类中的方法。是因为所有被代理执行的方法，都是通过在InvocationHandler中的invoke方法调用的，所以我们只要在invoke方法中统一处理，就可以对所有被代理的方法进行相同的操作了。

