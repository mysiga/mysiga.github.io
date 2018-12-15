title: Android面试
date: 2016-08-24 15:47:29
tags:
categories: Android
---


面试问题总结
## Android
#### Handler机制
- [Handler机制详解](http://www.jianshu.com/p/a04cbabaec83)
- [Android-异步通信Handler机制](http://www.jianshu.com/p/041fff5acdcf)画图更清晰
- [Android异步消息处理机制完全解析，带你从源码的角度彻底理解](http://blog.csdn.net/guolin_blog/article/details/9991569)

#### 自定义View
先总的分类:自绘控件、组合控件、继承控件
- [Android自定义View的实现方法，带你一步步深入了解View(四)](http://blog.csdn.net/guolin_blog/article/details/17357967)

在回头看View绘制源码
- [Android LayoutInflater原理分析，带你一步步深入了解View(一)](http://blog.csdn.net/guolin_blog/article/details/12921889)
- [Android视图绘制流程完全解析，带你一步步深入了解View(二)](http://blog.csdn.net/guolin_blog/article/details/16330267)
- [Android视图状态及重绘流程分析，带你一步步深入了解View(三)](http://blog.csdn.net/guolin_blog/article/details/17045157)
注：除了自绘控件需要搞清楚三个方法(onMeasure, onLayout, onDraw)外其他两个自定义相对比较简单。

#### 事件分发机制
- [Android事件分发机制完全解析，带你从源码的角度彻底理解(上)](http://blog.csdn.net/guolin_blog/article/details/9097463)
- [Android事件分发机制完全解析，带你从源码的角度彻底理解(下)](http://blog.csdn.net/guolin_blog/article/details/9153747)
- [图解 Android 事件分发机制](http://www.jianshu.com/p/e99b5e8bd67b)
注:三个重要方法:
    - dispatchTouchEvent事件分发
    - onInterceptTouchEvent 事件拦截
    - onTouchEvent() 事件处理

#### 跨进程通信IPC
- 序列化
  - Serializable：对象序列化到存储设备或者将对象序列化通过网络传输优先选择
  - Parcelable:Android首选
- Bindler
- IPC方式
  - Bundle
  - 文件共享
  - Messenger(不是handler的Message)

- AIDL
  - [Android Service完全解析，关于服务你所需知道的一切(上)](http://blog.csdn.net/guolin_blog/article/details/11952435)
  - [Android Service完全解析，关于服务你所需知道的一切(下)](http://blog.csdn.net/guolin_blog/article/details/9797169)

#### Android适配
- [Android开发：最全面、最易懂的Android屏幕适配解决方案](http://www.jianshu.com/p/ec5a1a30694b)
- [Android切图](http://www.ui.cn/detail/79573.html)
    - 1套标注dp图
    - 三套切图(hdpi,xhpi,xxhdpi)

#### Android动画
 - Frame Animation(帧动画)：按顺序播放事先做好的图像
 - Tween Animation(补间动画):只能对View进行平移，缩放，渐变，旋转
 - Property Animation(属性动画):API11开始引入，基于Object进行属性改变而
达到动画效果
 - 使用动画场景
    - 转场动画
    - 加载动画
    - 其他动画

## Java基础
- ["=="和equals区别](http://www.cnblogs.com/zhxhdean/archive/2011/03/25/1995431.html)
  "=="是比较的内存地址，equals比较的是值。
- [Java内存](http://wiki.jikexueyuan.com/project/java-vm/storage.html)
    ![Java内存区域](http://upload-images.jianshu.io/upload_images/1534431-14bf0620b5705bf7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 程序计数器
  - Java 虚拟机栈
      1. 局部变量表
      > 局部变量表是一组变量值存储空间，用于存放方法参数和方法内部定义的局部变量，其中存放的数据的类型是编译期可知的各种基本数据类型、对象引用（reference）和 returnAddress 类型（它指向了一条字节码指令的地址）.
      
      2. 操作数栈
      > 操作数栈又常被称为操作栈，操作数栈的最大深度也是在编译的时候就确定了。
      3. 动态连接
        > 每个栈帧都包含一个指向运行时常量池（在方法区中，后面介绍）中该栈帧所属方法的引用，持有这个引用是为了支持方法调用过程中的动态连接。
      4. 方法返回地址
      > 当一个方法被执行后，有两种方式退出该方法：执行引擎遇到了任意一个方法返回的字节码指令或遇到了异常，并且该异常没有在方法体内得到处理。无论采用何种退出方式，在方法退出之后，都需要返回到方法被调用的位置，程序才能继续执行。
- 本地方法栈
该区域与虚拟机栈所发挥的作用非常相似，只是虚拟机栈为虚拟机执行 Java 方法服务，而本地方法栈则为使用到的本地操作系统（Native）方法服务。
- Java 堆
Java Heap 是 Java 虚拟机所管理的内存中最大的一块，它是所有线程共享的一块内存区域。几乎所有的对象实例和数组都在这类分配内存。Java Heap 是垃圾收集器管理的主要区域，因此很多时候也被称为“GC堆”。
- 方法区
方法区也是各个线程共享的内存区域，它用于存储已经被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据
- 直接内存
   不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范定义的内存区域(Fresco图片加载库，就是运行这块内存超越虚拟机内存使用)。
> 在 JDK1.4 中新引入了 NIO 机制，它是一种基于通道与缓冲区的新 I/O 方式，可以使用Native函数库直接分配堆外内存，然后通过存储在Java堆里面的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中提高性能，因为避免了在 Java 堆和 Native 堆中来回复制数据。
- 参考文章
  - [Java类生命周期](http://blog.csdn.net/zhengzhb/article/details/7517213)
  - [单例模式讨论篇：单例模式与垃圾回收](http://blog.csdn.net/zhengzhb/article/details/7331354)

- int-char-long各占多少字节数

| 类型 | 位数 | 字节 |
| :------  | :------ | :------ |
| byte    | 8       | 1       |
| char    | 16     | 2     |
| short   | 16     | 2       |
| int       | 32     | 4       |
| long    | 64     | 8      |
| float    | 32     | 4       |
| double| 64     | 8      |

  - [Java 23种设计模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
    - 单例模式
    - [工厂模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
       1. [静态工厂模式](http://www.cnblogs.com/lcw/p/3802790.html)
       2. [工厂方法模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
       3. [抽象工厂模式](http://wiki.jikexueyuan.com/project/java-design-pattern/abstract-factory-pattern.html)
       4. 区别：
          > 静态工厂模式:用静态方法获取指定产品
             工厂方法模式:定义工厂接口,一个方法,让不同的子类工厂生产指定产品
             抽象工厂模式:同工厂方法模式类似,只是工厂接口有多个方法
    - 建造者模式
    - 代理模式
    - 策略模式
    - 模板模式
    - 观察者模式

#### 网络
- [HTTP,TCP,Socket关系](http://blog.csdn.net/bjyfb/article/details/6682913)
 - [TCP和UDP区别？TCP几次握手，几次断开？](http://mp.weixin.qq.com/s?__biz=MzA4MzQxMzg5MA==&mid=2649830415&idx=2&sn=760da2138f07c96ef25d451d745c4c72&scene=0#wechat_redirect)

#### 参考资料
- [Android 开发工程师面试指南](http://www.diycode.cc/wiki/androidinterview)