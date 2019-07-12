# 前言
并发大师Doug Lea

## 00. 并发编程
![image](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/00.xmind.png)
## 01. 并发编程的bug源头
	CPU、内存、I/O 三者速度的差异
	1. CPU增加了缓存，以均衡与内存的速度差异；
	2. 操作系统增加了进程、线程、以分时复用CPU，进而均衡CPU与I/O设备的速度差异；
	3 编译程序优化指令执行次序，使得缓存能够够得到更加合理地利用
	
### 源头一：缓存导致的可见性问题
![CPU缓存与内存的关系-单核](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.%E5%8D%95CPU.png)
![CPU缓存与内存的关系-多核](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.%E5%A4%9ACPU.png)
	
### 源头二：线程切换带来的原子性问题
	
我们把一个或多个操作在CPU执行过程中不被中断的特性称为原子性
count += 1
* 指令1：首先，需要把变量count从内存加载到CPU的寄存器
* 指令2：之后，在寄存器中执行+1操作
* 指令3：最后，将结果写入内存（缓存机制导致可能写入的是CPU缓存而不是内存）
![线程切换](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.%E7%BA%BF%E7%A8%8B%E5%88%87%E6%8D%A2.png)
![非原子操作执行路径示意图](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.%E9%9D%9E%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C.png)
	
### 源头三：编译优化带来的有序性问题
	编译器为了优化性能，有时候会改变程序中语句的先后顺序
	举例：双重教验锁
``` stylus
public class Singleton {
  static Singleton instance;
  static Singleton getInstance(){
    if (instance == null) {
      synchronized(Singleton.class) {
        if (instance == null)
          instance = new Singleton();
        }
    }
    return instance;
  }
}
```
	new 操作：
	1.分配一块内存M；
	2.在内存M上初始化Singleton对象
	3.然后M的地址赋值给instance变量
	
	但是实际上优化后的执行路径却可以是这样的：
	
	1.分配一块内存M；
	2.M的地址赋值给instance变量；
	3.在内存M上初始化Singleton对象
![编译器优化](https://github.com/xaoduer/learning/blob/master/java-learning/java-concurrent-png/01.%E7%BC%96%E8%AF%91%E5%99%A8%E4%BC%98%E5%8C%96%E5%B8%A6%E6%9D%A5%E7%9A%84%E9%97%AE%E9%A2%98.png)

解决方案，volatile（写入屏障），禁止编译优化，禁止指令重排序，线程A执行完后会强制刷新到内存，线程B强制把内存中的内容写入到自己的缓存中
	
