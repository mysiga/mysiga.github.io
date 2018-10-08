title: 深入Java线程
date: 2018-08-02  18:30:00
tags:
categories: Java
---


线程是系统调度的最小单元，一个进程可以包含多个线程，作为任务的真正运作者，有自己的栈（Stack）、寄存器（Register）、本地存储（Thread Local）等，但是会和进程内其他线程共享文件描述符、虚拟地址空间等。在具体实现中，线程还分为内核线程、用户线程，Java 的线程实现其实是与虚拟机相关的。
### 创建线程方式
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
### 常用锁处理
  - [synchronized](https://www.jianshu.com/p/d53bf830fa09)  
  
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
  
### volatile和synchronized的区别

| | volatile | synchronized  | 
| ----- | ----- | ----- | 
| 作用域 | 变量 | 方法，代码块 | 
| 操作性| 仅保证可见性| 可以保证可见性和原子性  | 
| 线程堵塞状态| 不会造成线程的阻塞| 可能会造成线程的阻塞  | 

### Lock和synchronized的区别
| | Lock | synchronized  | 
| ----- | ----- | ----- | 
| 使用区别 | 类 | 关键字  | 
| 作用域| 代码块| 代码块，方法 | 
| 释放锁| 需要finally()方法手动释放锁| 执行完自动释放锁  | 
| 锁类型| 可重入,可判断,可公平（两者皆可）|可重入,不可中断,非公平| 
| 依托实现| Java代码控制| JVM执行 | 
|性能|高竞争场景中表现可能优于synchronized|在低竞争场景中表现可能优于 ReentrantLock|
### sleep()和wait()的区别
| | sleep | wait  | 
| ----- | ----- | ----- | 
|| 睡眠时，保持对象锁，仍然占有该锁 | 睡眠时，释放对象锁  | 
| 作用域| 任何地方调用 | 只能在同步方法或同步块中使用| 
| 时间| 指定时间后唤醒 | 只有等notify()/notifyAll()通知有才能唤醒| 

### 并发高效类
实际开发中Java封装了一些解决高并发的类包（java.util.concurrent包），常用的如下：

- AtomicInteger：提供一种线程安全的加减操作
- AtomicBoolean
- AtomicLong
- AtomicReference
- AtomicIntegerArray
- AtomicLongArray
- AtomicReferenceArray

### 线程生命周期
![线程生命周期](https://upload-images.jianshu.io/upload_images/1534431-8837ea32b7cd19b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

### 线程安全基本特性
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
![image.png](https://upload-images.jianshu.io/upload_images/1534431-1816e7d12c4ed6c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

### [线程池](https://mp.weixin.qq.com/s/eyFVplFcZoN6-4WTMojtMw)
Executors 目前提供了 5 种不同的线程池创建配置：

- newCachedThreadPool()，它是一种用来处理大量短时间工作任务的线程池，具有几个鲜明特点：它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；如果线程闲置的时间超过 60 秒，则被终止并移出缓存；长时间闲置时，这种线程池，不会消耗什么资源。其内部使用 SynchronousQueue 作为工作队列。
- newFixedThreadPool(int nThreads)，重用指定数目（nThreads）的线程，其背后使用的是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会有新的工作线程被创建，以补足指定的数目 nThreads。
- newSingleThreadExecutor()，它的特点在于工作线程数目被限制为 1，操作一个无界的工作队列，所以它保证了所有任务的都是被顺序执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避免其改变线程数目。
- newSingleThreadScheduledExecutor() 和 newScheduledThreadPool(int corePoolSize)，创建的是个 ScheduledExecutorService，可以进行定时或周期性的工作调度，区别在于单一工作线程还是多个工作线程。
- newWorkStealingPool(int parallelism)，这是一个经常被人忽略的线程池，Java 8 才加入这个创建方法，其内部会构建ForkJoinPool，利用Work-Stealing算法，并行地处理任务，不保证处理顺序。

线程池构造方法几个参数

```
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
```
1. corePoolSize : 该线程池中核心线程数最大值
核心线程：线程池新建线程的时候，如果当前线程总数小于 corePoolSize ，则新建的是核心线程；如果超过corePoolSize，则新建的是非核心线程。核心线程默认情况下会一直存活在线程池中，即使这个核心线程啥也不干(闲置状态)。如果指定ThreadPoolExecutor的 allowCoreThreadTimeOut 这个属性为true，那么核心线程如果不干活(闲置状态)的话，超过一定时间( keepAliveTime)，就会被销毁掉.
2. maximumPoolSize ：该线程池中线程总数的最大值
线程总数计算公式 = 核心线程数 + 非核心线程数。
3. keepAliveTime  ：该线程池中非核心线程闲置超时时长
注意：一个非核心线程，如果不干活(闲置状态)的时长，超过这个参数所设定的时长，就会被销毁掉。但是，如果设置了  allowCoreThreadTimeOut = true，则会作用于核心线程。
4. unit ：（时间单位）
首先，TimeUnit是一个枚举类型，翻译过来就是时间单位，我们最常用的时间单位包括：
MILLISECONDS ： 1毫秒 、SECONDS ： 秒、MINUTES ： 分、HOURS ： 小时、DAYS ： 天
5. BlockingQueue<Runnable> workQueue ：( Blocking：阻塞的，queue：队列)，主要有四种队列
   -  SynchronousQueue：（同步队列）这个队列接收到任务的时候，会直接提交给线程处理，而不保留它（名字定义为 同步队列），所有该队列跟设置的corePoolSize无效。但有一种情况，假设所有线程都在工作怎么办？这种情况下，SynchronousQueue就会新建一个线程来处理这个任务。所以为了保证不出现（线程数达到了maximumPoolSize而不能新建线程）的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大，去规避这个使用风险。
    - LinkedBlockingQueue（链表阻塞队列）：这个队列接收到任务的时候，如果当前线程数小于核心线程数，则新建线程(核心线程)处理任务；如果当前线程数等于核心线程数，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize
    - ArrayBlockingQueue（数组阻塞队列）：可以限定队列的长度（既然是数组，那么就限定了大小），接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误
    - DelayQueue（延迟队列）：队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务
6. ThreadFactory threadFactory = > 创建线程的方式，这是一个接口，new它的时候需要实现他的Thread newThread(Runnable r)方法
7. RejectedExecutionHandler handler = > 这个主要是用来抛异常的
当线程无法执行新任务时（一般是由于线程池中的线程数量已经达到最大数或者线程池关闭导致的），默认情况下，当线程池无法处理新线程时，会抛出一个RejectedExecutionException。

以 LinkedBlockingQueue、ArrayBlockingQueue 和 SynchronousQueue 为例，我们一起来分析一下，根据需求可以从很多方面考量：

### ArrayBlockingQueue ,LinkedBlockingQueue ,SynchronousQueue 
- 从空间利用角度，数组结构的 ArrayBlockingQueue 要比 LinkedBlockingQueue 紧凑，因为其不需要创建所谓节点，但是其初始分配阶段就需要一段连续的空间，所以初始内存需求更大。
- 通用场景中，LinkedBlockingQueue 的吞吐量一般优于 ArrayBlockingQueue，因为它实现了更加细粒度的锁操作。
- ArrayBlockingQueue 实现比较简单，性能更好预测，属于表现稳定的“选手”。
- 如果我们需要实现的是两个线程之间接力性（handoff）的场景，按照专栏上一讲的例子，你可能会选择 CountDownLatch，但是SynchronousQueue也是完美符合这种场景的，而且线程间协调和数据传输统一起来，代码更加规范。
- 可能令人意外的是，很多时候 SynchronousQueue 的性能表现，往往大大超过其他实现，尤其是在队列元素较小的场景。

### 重点源码分享
okhttp

```
 public synchronized ExecutorService executorService() {
    if (executorService == null) {
      executorService = new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60, TimeUnit.SECONDS,
          new SynchronousQueue<Runnable>(), Util.threadFactory("OkHttp Dispatcher", false));
    }
    return executorService;
  }
```
```
 synchronized void enqueue(AsyncCall call) {
    if (runningAsyncCalls.size() < maxRequests && runningCallsForHost(call) < maxRequestsPerHost) {
      runningAsyncCalls.add(call);
      executorService().execute(call);
    } else {
      readyAsyncCalls.add(call);
    }
  }
```
AsyncTask

```
 private static final BlockingQueue<Runnable> sPoolWorkQueue =
            new LinkedBlockingQueue<Runnable>(128);
    /**
     * An {@link Executor} that can be used to execute tasks in parallel.
     */
    public static final Executor THREAD_POOL_EXECUTOR;
    static {
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
                CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE_SECONDS, TimeUnit.SECONDS,
                sPoolWorkQueue, sThreadFactory);
        threadPoolExecutor.allowCoreThreadTimeOut(true);
        THREAD_POOL_EXECUTOR = threadPoolExecutor;
    }
```

```
private static class SerialExecutor implements Executor {
        final ArrayDeque<Runnable> mTasks = new ArrayDeque<Runnable>();
        Runnable mActive;

        public synchronized void execute(final Runnable r) {
            mTasks.offer(new Runnable() {
                public void run() {
                    try {
                        r.run();
                    } finally {
                        scheduleNext();
                    }
                }
            });
            if (mActive == null) {
                scheduleNext();
            }
        }

        protected synchronized void scheduleNext() {
            if ((mActive = mTasks.poll()) != null) {
                THREAD_POOL_EXECUTOR.execute(mActive);
            }
        }
    }
```






