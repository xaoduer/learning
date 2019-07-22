# GC

## 如何判断对象死亡
引用计数法  
可达性分析法  
GC Roos :  
* 虚拟机栈（栈帧中的本地变量表）中引用的对象
* 方法区中类静态属性，常量引用的对象  
* 本地方法栈中JNI（native方法）引用的对象

## GC算法
### 复制  Copying  新生代
### 标记-清楚 Mark-Sweep 老年代
### 标记-整理 Mark-Sweep 老年代
### 分代收集

## GC收集器
### Young generation  主要采用复制算法
#### Serial —— 单线程 STW
#### ParNew —— Serial的多线程版本，支持多个线程同时进行垃圾收集 STW
#### Parallel Scavenge  —— 吞吐量=用户代码/(用户代码+垃圾收集代码)时间，与ParNew一样，但更关注吞吐量。老年代只能选择Serial Old，无法与CMS搭配

### Tenured generation 主要采用标记-整理算法
#### Serial Old —— 单线程 STW
#### Parallel Old —— PS的老年代版本，主要搭配PS，吞吐量优先
#### CMS —— 为了减少STW时间，并发标记
* 初始标记 STW 标记GC Roots能直接关联到的对象，单线程
* 并发标记 GC Roots Tracing 并发
* 重新标记 STW 修正变动
* 并发清除 

### G1 Garbage First 化整为零Region
#### 特点
* 并行与并发  
* 分代
* 空间整合
* 可预测的停顿

#### 步骤
* 初始标记 STW 标记GC Roots能直接关联到的对象，单线程
* 并发标记 Remembered Set Logs
* 最终标记 STW 合并Remembered Set Logs到Remembered Set
* 筛选回收 