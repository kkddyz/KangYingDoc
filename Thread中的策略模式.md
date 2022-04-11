## Thread类中的策略模式



在我学习Lambda的时候，我使用一个词 *"代码块注入"*来描述Lambda的作用（事实也正是如此） 。

注入这个词相当优雅。



### 两种创建线程的方式

学过Thread类的都知道创建一个有两种方式

```java
// 1. 通过继承Thread类，重写run()
public class MyThread extends Thread{
	@Override
    public void run() {
        System.out.println("通过继承Thread类，重写run(),创建新线程");
    }

}

// 2. 传入实现Runnable接口的对象 
Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("注入方法代码");
            }
        });
```



> 补充一下：Thread类的细节 ,只是伪码。
>
> start的方法签不没写，只有内部逻辑代码
>
> ```java
> public class Thread extends Runnable{
> 	private Runnable r;
> 
> 	start(){
>      // do something before 
> 
> 		if (this.r = null){ /*没有注入代码*/
> 
>      	this.run(); // 调用自己的run()
> 
>  	}else{ /*传入接口对象*/
> 
>          r.run(); // 调用传入的run()
>      }
>  };
> 
>  @Override 
>  public void run(){
>      // 默认空方法体
>  }
> }
> 
> ```
>
> 可以发现，如果我们没有使用第二种方法，那么私有成员r就是默认的null。
>
> 这时候就会调用重写的run()，而不是调用r.run();





### :star:那么这两种方式有什么区别？？

在分析他们的区别之前，我们先分析这两种方式的共同目的 :

- **为Thread类留下一定的弹性空间，以应对客户变化的需求。**

如何理解这句话，那我们得先理解Thread类的作用。 创建一个线程？ 

不，更正确的说法是：为使用者提供创建线程的所有准备。

因为我们还需要提供线程需要的*执行代码*，这就是所谓**变化的需求**。



在程序中，变化意味着不确定性，意味着程序必须要有弹性。 



#### 第一种应对变化的方式 ：**继承**

```JAVA
public abstract class Thread {
    start(){...};
    abstract void run();
}

```

所有Thread子类通过覆盖run() 确保start()的正确执行。

但是这种方式有很大的问题，原因在与它把代码写入类中； (将实现绑定在类中)

每一次变化的需求都对应一个类。这会会导致 *"类爆炸"* （这是滥用继承一定会出现的问题）。 



思考一下，类的模式没有发生改变(接口不变)，改变的只是方法的实现罢了。

如果这里的方法是一个变量，问题就很简单了。

> int var ;
>
> System.out.println(var);

这里的模式都是输出一个值，var作为变量可以承载不同的值。

那么有没有可能存在一个对象，它可以存放一些代码。

有，就是接口啊。将接口作为参数，目的是传入方法块，这正是Thread创建的第二种方式。

---

#### 第二种应对变化的方式 ： "**组合**" 

Thread类需要执行一段代码，这段代码保存在*接口对象*这个‘‘变量’’中，我们通过赋值可以动态决定到底放入什么代码。这就是所谓的注入。



> 多用组合，少用继承； 设计原则 -- 参考headfirst设计模式 第一章 23页



确实就是策略模式的解决思路。



所以二者的区别，其实就是继承与组合在面对变化的时候的区别。



> 使用lambda简化注入代码块的过程。



