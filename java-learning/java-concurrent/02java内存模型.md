# Java内存模型
解决可见性、有序性的最直接办法就是按需禁用缓存和编译优化
Java内存模型规范了JVM如何提供按需禁用缓存和编译优化的方法。具体来说，这些方法包括volatile、synchronized和final三个关键字，以及六项Happens-Before规则
Java内存模型在1.5版本对volatile语义进行了增强，怎么增强的呢？答案是Happens-Before规则

## Happens-Before真正要表达的是：前面一个操作的结果对后续操作是可见的
并不是说前面一个操作发生在后续操作的前面，Happens-Before约束了编译器的优化行为，虽允许编译器优化，但是要求编译器优化后一定遵守Happens-Before规则

### 程序的顺序性规则
指在一个线程中，按照程序顺序，前面的操作Happens-Before于后续的任意操作
### volatile变量规则
指一个对volatile变量的写操作，Happens-Before于后续对这个volatile变量的读操作
### 传递性
如果A Happens-Before B,q且B Happens-Before C,那么A Happens-Before C

``` stylus
// 以下代码来源于【参考 1】
class VolatileExample {
  int x = 0;
  volatile boolean v = false;
  public void writer() {
    x = 42;
    v = true;
  }
  public void reader() {
    if (v == true) {
      // 这里 x 会是多少呢？
    }
  }
}
``` 

![传递性](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.transmissibility.png)
1.“x=42” Happens-Before 写变量 “v=true",这是规则1的内容;
2.写变量“v=true” Happens-Before 读变量 "v=true",这是规则2的内容

### 管程中锁的规则
对一个锁的解锁Happens-Before于后续对这个锁的加锁
管程在 java中指的就是synchronized，synchronized是java里对管程的实现
### 线程start()规则
关于线程启动的，指主线程A启动子线程B后，子线程B能够看到主线程在启动子线程B前的操作
换句话说，如果线程A调用线程B的start()方法（即在线程A中启动线程B），那么该start()操作Happens-Before于线程B中的任意操作
``` stylus
Thread B = new Thread(()->{
  // 主线程调用 B.start() 之前
  // 所有对共享变量的修改，此处皆可见
  // 此例中，var==77
});
// 此处对共享变量 var 修改
var = 77;
// 主线程启动子线程
B.start();
``` 
### 线程Join()规则
这是关于线程等待的，指主线程A等待子线程B完成（主线程通过调用子线程B的join()方法实现），当子线程B完成后（主线程A中join()方法返回），主线程能够看到子线程的操作（指对共享变量的操作）。
换句话说，如果在线程A中，调用线程B的join()并返回成功，那么线程B中的任意操作Happens-Before于该join()操作的返回。
``` stylus
Thread B = new Thread(()->{
  // 此处对共享变量 var 修改
  var = 66;
});
// 例如此处对共享变量修改，
// 则这个修改结果对线程 B 可见
// 主线程启动子线程
B.start();
B.join()
// 子线程所有对共享变量的修改
// 在主线程调用 B.join() 之后皆可见
// 此例中，var==66
``` 
### final 生而不变
构造函数溢出，线程通过global.obj读取x有可能读到0
``` stylus
// 以下代码来源于【参考 1】
final int x;
// 错误的构造函数
public FinalFieldExample() { 
  x = 3;
  y = 4;
  // 此处就是讲 this 逸出，
  global.obj = this;
}
``` 

