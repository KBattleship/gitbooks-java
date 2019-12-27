# 5.单例模式
<font style="color:red;">**单例模式：**</font>`保证任何情况下都绝对的只生成一个实例的模式。`
#### ①.单例角色
+ `Singleton`: 有且仅有的一个角色(实例)。

#### ②.实现单例
+ **方式一：**简化实现的单例模式

```java
public class Singleton {
    private static Singleton singleton=null;
    /*
        私有化，不给外界直接new的机会
     */
    private Singleton() {
    }
    /*
        1.简化实现的单例模式，但遇到多线程肯定有问题
     */
    public static Singleton getInstance(){
        if (singleton==null){
            singleton=new Singleton();
        }
        return singleton;
    }
}
```

+ **方式二：**加锁控制多线程
+ **方式三：**懒汉模式
+ **方式四：**双重加锁机制