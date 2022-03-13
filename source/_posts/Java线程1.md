title: 深入Java线程(一)
date: 2018-08-02  18:30:00
tags:
categories: Java
---


线程是系统调度的最小单元，一个进程可以包含多个线程，作为任务的真正运作者，有自己的栈（Stack）、寄存器（Register）、本地存储（Thread Local）等，但是会和进程内其他线程共享文件描述符、虚拟地址空间等。在具体实现中，线程还分为内核线程、用户线程，Java 的线程实现其实是与虚拟机相关的。
###创建线程方式
  - 继承Thread
```
public class ThreadImpl extends Thread {   
}
```
  - 实现Runable
```
public class MyThread implements Runnable {
    private int ticket;
    private Lock lock = new ReentrantLock();

    public MyThread(int ticket) {
        this.ticket = ticket;
    }

    @Override
    public void run() {
        print();
    }

    public void print() {
        lock.lock();
        try {
            while (ticket > 0) {
                System.out.println("ticket==" + ticket);
                ticket--;
            }
        } catch (Exception e) {
            System.out.println("Exception==" + e);
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        MyThread myThread = new MyThread(5);
        new Thread(myThread).start();
        new Thread(myThread).start();
    }
}
```
  - 实现Callable，运行完成后可返回值。
```
public class CallTask implements Callable {
    private int upperBounds;

    public CallTask(int upperBounds) {
        this.upperBounds = upperBounds;
    }

    @Override
    public Integer call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= upperBounds; i++) {
            sum += i;
        }
        return sum;
    }

    public static void main(String[] args) {
        ExecutorService service = Executors.newFixedThreadPool(10);
        try {
            int sum = (int) service.submit(new CallTask((int) (Math.random() * 100))).get();
            System.out.println("sum==" + sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```
- [结束线程的三种方法](https://blog.csdn.net/xu__cg/article/details/52831127)
  - run()方法执行完正常退出
  - 调用interrupt()方法
  - 使用stop方法强行终止线程（不推荐使用，Thread.stop, Thread.suspend, Thread.resume 和Runtime.runFinalizersOnExit 这些终止线程运行的方法已经被废弃，使用它们是极端不安全的！）
###常用锁处理
  - [synchronized](https://www.jianshu.com/p/d53bf830fa09)  :
    - 对象锁：非静态synchronized方法，同个对象相互排斥，不同类对象不互斥
    - 类锁：静态synchronized方法和synchronized代码块方法, 不同类对象互斥，同个对象也互斥。
```
public class LockThread implements Runnable {    
  private Integer key = 0;      
  @Override    
  public void run() {       
     synchronized (this) {    
        key++;          
     System.out.println(Thread.currentThread().getName() + ":" + key);    
    try {        
        Thread.sleep(5);   
       } catch (InterruptedException e) {        
        e.printStackTrace();    }
      }
   }    
public static void main(String[] args) {       
 LockThread lockThread = new LockThread();       
 for (int i = 0; i < 100; i++) {
        new Thread(lockThread, "thread" + i).start();        
    }    
   }
}
```
  - lock
```
public class LockThread implements Runnable {    
  private Integer key = 0;    
  private Lock lock = new ReentrantLock();    
  @Override    
  public void run() {       
     lock.lock();  //获取锁     
       try {           
           key++;                
          System.out.println(Thread.currentThread().getName() + ":" + key);            
          Thread.sleep(5);       
          } catch (Exception e) {        
        } finally {            
        lock.unlock();  //释放锁     
       }    
     }    
public static void main(String[] args) {       
 LockThread lockThread = new LockThread();       
 for (int i = 0; i < 100; i++) {            
       new Thread(lockThread, "thread" + i).start();        
      }    
     }
}
```
  - volatile
  ###volatile和synchronized的区别

| | volatile | synchronized  | 
| ----- | ----- | ----- | 
| 作用域 | 变量 | 方法，代码块 | 
| 操作性| 仅保证可见性，有序性| 可以保证可见性和原子性  | 
| 线程堵塞状态| 不会造成线程的阻塞| 可能会造成线程的阻塞  | 

  ###Lock和synchronized的区别
| | Lock | synchronized  | 
| ----- | ----- | ----- | 
| 使用区别 | 类 | 关键字  | 
| 作用域| 代码块| 代码块，方法 | 
| 释放锁| 需要finally()方法手动释放锁| 执行完自动释放锁  | 
| 锁类型| 可重入,可判断,可公平（两者皆可）|可重入,不可中断,非公平| 
| 依托实现| Java代码控制| JVM执行 | 
|性能|高竞争场景中表现可能优于synchronized|在低竞争场景中表现可能优于 ReentrantLock|
###sleep()和wait()的区别
| | sleep | wait  | 
| ----- | ----- | ----- | 
|| 睡眠时，保持对象锁，仍然占有该锁 | 睡眠时，释放对象锁  | 
| 作用域| 任何地方调用 | 只能在同步方法或同步块中使用| 
| 时间| 指定时间后唤醒 | 只有等notify()/notifyAll()通知有才能唤醒| 
###并发高效类
实际开发中Java封装了一些解决高并发的类包（java.util.concurrent包），常用的如下：
- AtomicInteger
提供一种线程安全的加减操作
- AtomicBoolean
- AtomicLong
- AtomicReference
- AtomicIntegerArray
- AtomicLongArray
- AtomicReferenceArray






