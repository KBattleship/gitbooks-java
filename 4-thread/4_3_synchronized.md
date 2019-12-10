# 4-3.Synchronized
---
## 一、 Synchronized的作用范围：适用于**方法**以及**代码块**中。

| 类型 | 具体类型 | 作用对象 | 代码示例|
|:-----:|----|:----:|----|
|方法|实例方法|类的实例对象|`public synchronized void method(){`<br>`}`|
|方法|静态方法|类对象|`public static synchronized void method(){`<br>`}`|
|代码块|实例对象|类的实例对象|`synchronized(this){`<br>`//同步代码块，锁住的是此类的实例对象;`<br>`}`|
|代码块|Class对象|类对象|`synchronized(SynchronizedDemo.class){`<br>`//锁住的是此类的实例对象;`<br>`}`|
|代码块|任意实例对象|任意实例对象|`String demo='synchronized';`<br>`synchronized(demo){`<br>`//锁住的为demo对象;`<br>`}`|
`Tips: 如果Synchronized锁作用于`**类对象**`,不管多少个new实例对象,它们终究属于此类,都会被锁住,即线程之间保证同步关系.`

---

## 二、Synchronized锁原理分析
### **`当一个线程访问同步代码块时，它首先是需要得到锁才能执行同步代码，当退出或者抛出异常时必须要释放锁。`**

### 1.通过javap命令查看生成的字节码文件进行分析Synchronized锁的实现:
```java
public class SynchronizedTest {
    public synchronized void demo(){

    }

    public void demoTwo(){
        synchronized (this){

        }
    }
}
```
将上述代码片段进行编译，使用<b>`javap -v  SynchronizedTest.class`</b>命令进行详细信息查看（摘录部分信息如下）:
```java
{
 public SynchronizedTest();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1    // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0

  public synchronized void demo();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=0, locals=1, args_size=1
         0: return
      LineNumberTable:
        line 4: 0

  public void demoTwo();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=3, args_size=1
         0: aload_0
         1: dup
         2: astore_1
         3: monitorenter  //监视器进入，获取锁
         4: aload_1
         5: monitorexit   //监视器退出，释放锁
         6: goto          14
         9: astore_2
        10: aload_1
        11: monitorexit
        12: aload_2
        13: athrow
        14: return
}
```

### 2.从以上信息中可以看出：

- Synchronized锁作用于<b>`代码块`</b>时是通过<b>`monitorenter`</b>和<b>`monitorexit`</b>指令进行实现的，<b>`Monitor`</b>对象是同步的基本实现单元:
    <br>

    `
       ①.monitorenter指令插入在同步代码段开始的位置,monitorexit指令插入在同步代码段结束的位置。JVM需要保证每一个monitorenter与monitorexit都是一一对应的关系。
    `
    <br>

    `
    ②.任何对象都会存在一个对应的monitor,当这个monitor被占有,将进入锁定状态（即获取锁）使Synchronized锁进行同步，当线程获取monitor后才能继续往下执行，否则就只能等待(获取monitor的过程是互斥的，即同一时刻只有一个线程能够获取到monitor)。
    `
- Synchronized锁作用于<b>`方法`</b>时是通过在方法修饰符上的<b>`ACC_SYNCHRONIZED`</b>标志实现的:
    <br>
    `
    Synchronized作用于方法时，会被编译成普通方法的调用和返回指令，在VM字节码层并没有任何特别的指令来实现被synchronized修饰的方法，但是在Class文件的方法表中将该方法的access_flags字段中的synchronized标志位置1,表示该方法是同步方法并使用调用该方法的对象或该方法所属的Class在JVM的内部对象表示Klass做为锁对象。
    `
---
## 三、关于Monitor和对象头
### 1.聊聊Monitor
Monitor（内部锁），我们可以理解成为是一个同步工具,也可以描述成一种同步机制。与一切皆对象一样，所有的Java对象是天生的Monitor，每一个Java对象都有成为Monitor的潜质，因为在Java的设计中 ，每一个Java对象自打娘胎里出来就带了一把看不见的锁，它叫做内部锁或者Monitor锁。

- 在Java6版本以前，Monitor的实现完全是依赖于操作系统内部的互斥锁,犹豫需要进行用户态到系统内核态的切换，所以同步操作是一个无差别的重量级操作。

- 但是在目前版本的JDK中，JVM进行了调整，提供有三种不同的Monitor实现(即常见的三类锁):**偏斜锁**(Biased Locking)、**轻量级锁**和**重量级锁**。