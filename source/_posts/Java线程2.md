title: 深入Java线程(二)
date: 2018-08-21  15:16:00
tags:
categories: Java
---


为了理解可先看[深入Java线程（一）](https://www.jianshu.com/p/5c7dd9bd506b)内容在看本篇。
###线程生命周期
![线程生命周期](/images/xianchengshengmingzhouqi.png)

关于线程生命周期的不同状态，在 Java 5 以后，线程状态被明确定义在其公共内部枚举类型 java.lang.Thread.State 中，分别是：
- 新建（NEW），表示线程被创建出来还没真正启动的状态，可以认为它是个 Java 内部状态。
- 就绪（RUNNABLE），表示该线程已经在 JVM 中执行，当然由于执行需要计算资源，它可能是正在运行，也可能还在等待系统分配给它 CPU 片段，在就绪队列里面排队。
在其他一些分析中，会额外区分一种状态 RUNNING，但是从 Java API 的角度，并不能表示出来。
- 阻塞（BLOCKED），这个状态和我们前面两讲介绍的同步非常相关，阻塞表示线程在等待 Monitor lock。比如，线程试图通过 synchronized 去获取某个锁，但是其他线程已经独占了，那么当前线程就会处于阻塞状态。如：sleep()
- 等待（WAITING），表示正在等待其他线程采取某些操作。一个常见的场景是类似生产者消费者模式，发现任务条件尚未满足，就让当前消费者线程等待（wait），另外的生产者线程去准备任务数据，然后通过类似 notify 等动作，通知消费线程可以继续工作了。Thread.join() 也会令线程进入等待状态。
- 计时等待（TIMED_WAIT），其进入条件和等待状态类似，但是调用的是存在超时条件的方法，比如 wait 或 join 等方法的指定超时版本，如下面示例：
```
public final native void wait(long timeout) throws InterruptedException;
```
- 终止（TERMINATED），不管是意外退出还是正常执行结束，线程已经完成使命，终止运行，也有人把这个状态叫作死亡。
在第二次调用 start() 方法的时候，线程可能处于终止或者其他（非 NEW）状态，但是不论如何，都是不可以再次启动的。

###线程安全基本特性
  - 原子性：简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现
  - 可见性:是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile就是负责保证可见性的。
 - 有序性，是保证线程内串行语义，避免指令重排等

### Monitor 实现的三种锁：
所谓锁的升级、降级，就是 JVM 优化 synchronized 运行的机制，当 JVM 检测到不同的竞争状况时，会自动切换到适合的锁实现，这种切换就是锁的升级、降级。
- 偏斜锁（Biased Locking）
当没有竞争出现时，默认会使用偏斜锁。JVM 会利用 CAS 操作（compare and swap），在对象头上的 Mark Word 部分设置线程 ID，以表示这个对象偏向于当前线程，所以并不涉及真正的互斥锁。这样做的假设是基于在很多应用场景中，大部分对象生命周期中最多会被一个线程锁定，使用偏斜锁可以降低无竞争开销。
- 轻量级锁
如果有另外的线程试图锁定某个已经被偏斜过的对象，JVM 就需要撤销（revoke）偏斜锁，并切换到轻量级锁实现。
- 重量级锁
轻量级锁依赖 CAS 操作 Mark Word 来试图获取锁，如果重试成功，就使用普通的轻量级锁；否则，进一步升级为重量级锁。

我注意到有的观点认为 Java 不会进行锁降级。实际上据我所知，锁降级确实是会发生的，当 JVM 进入安全点（SafePoint）的时候，会检查是否有闲置的 Monitor，然后试图进行降级
![image.png](/images/suojiangjiduibi.png)

- 可重入锁：它是表示当一个线程试图获取一个它已经获取的锁时,这个获取动作就自动成功。
ReentrantLock和synchronized，
```
public class RepeLock implements Runnable {
    private Lock lock = new ReentrantLock();
    public void get() {
        try {
            lock.lock();
            System.out.println(Thread.currentThread().getId());
            set();
        } finally {
            lock.unlock();
        }
    }
    public void set() {
        try {
            lock.lock();
            System.out.println(Thread.currentThread().getId());
        } finally {
            lock.unlock();
        }
    }

    @Override
    public void run() {
        get();
    }
    public static void main(String[] args) {
        RepeLock repeLock = new RepeLock();
        new Thread(repeLock).start();
        new Thread(repeLock).start();
        new Thread(repeLock).start();
    }
}
```
- 锁膨胀：
- 自旋锁：
线程去拿锁时发现已经有线程拿了锁，然后让线程去执行一个无意义的循环，循环结束后再去重新竞争锁，如果竞争不到继续循环，循环过程中线程会一直处于running状态，但是基于JVM的线程调度，会出让时间片，所以其他线程依旧有申请锁和释放锁的机会。自旋锁省去了阻塞锁的时间空间（队列的维护等）开销，但是长时间自旋就变成了“忙式等待”，忙式等待显然还不如阻塞锁。所以自旋的次数一般控制在一个范围内，例如10,100等，在超出这个范围后，自旋锁会升级为阻塞锁。
- [指令重排](https://www.cnblogs.com/chenyangyao/p/5269622.html)






