title: Android跨进程通信
date: 2021-09-07  19:20:00
tags:
categories: Android

---



#### 跨进程通信IPC
> RPC 即 Remote Procedure Call (远程过程调用) 是一种计算机通讯协议，它为我们定义了计算机 C 中的程序如何调用另外一台计算机 S 的程序，让程序员不需要操心底层网络协议，使得开发包括网络分布式多程序在内的应用程序更加容易。
IPC 即 Inter-Process Communication (进程间通信)，

Android 为我们提供了以下几种进程通信机制（供开发者使用的进程通信 API）对应的文章链接如下：
- intent
- 文件
通过将数据写入到一个文件中，不同进程可以对这个文件进行读取访问，来达到跨进程通信目的。 不过，多进程同时访问一个文件，存在并发和IO性能低的问题
- AIDL （基于 Binder）
[Android 进阶：进程通信之 AIDL 的使用](https://blog.csdn.net/u011240877/article/details/72765136)
```
1、定义跨进程Activity
2、定义默认进程Server
3、跟进Activity和server需要支持的接口定义AIDL接口
4、activity和server进行绑定并通过aidl接口进行通信
```
[Android 进阶：进程通信之 Binder 机制浅析](https://blog.csdn.net/u011240877/article/details/72801425)
[Android 进阶：进程通信之 AIDL 解析](https://blog.csdn.net/u011240877/article/details/72825706)
- Messenger （基于 Binder）
[Android 进阶：进程通信之 Messenger 使用与解析](https://blog.csdn.net/u011240877/article/details/72836178)
- ContentProvider （基于 Binder）
[Android 进阶：进程通信之 ContentProvider 内容提供者](https://blog.csdn.net/u011240877/article/details/72848608)
- Socket
[Android 进阶：进程通信之 Socket （启本地服务进行连接）](https://blog.csdn.net/u011240877/article/details/72860483)

#### 通信方式对比
|通信方式|优点|缺点|使用场景|
|---------|---------|---------|---------|
|intent(Bundle)|简单易用|只能传输Bundle支持的数据类型|四大组件的进程通信,Activity,Broadcast|
|文件共享|简单易用|不适合高并发场景，无法做到进程间的及时通信| 交换简单数据不需要并发|
|AIDL|功能强大，支持一对多的并发通信，支持实时通信|使用稍复杂，需要处理好线程同步| 一对多通信且有RPC需求|
|Message|功能一般，支持一对多的串行通信，支持一对多并发数据共享|不能很好处理并发现象，不支持RPC,只能传输Bundle支持的数据类型| 低并发的一对多即时通信，无RPC需求|
|ContentProvider|数据访问方面功能强大，支持一对多并发数据共享|受约束的AIDL,主要提供数据源的CRUD操作| 一对多的进程间的数据共享|
|Socket|功能强大，可通过网络传输字节流，支持一对多的并发实时通信| 繁琐，不支持直接的RPC|网络数据交换|

#### Binder 通信机制
- [Binder 通信机制](https://blog.csdn.net/carson_ho/article/details/73560642)
![image.png](/images/bindertongxinjizhi.png)
#### Binder 机制跨进程通信流程
![image.png](/images/binderkuanjinchengtongxin.png)

#### OSI7层模型与TCP/IP4层模型
![image.png](/images/osi7cengxieyi.png)

#### TCP/IP相关协议
![image.png](/images/tcpipxieyi.png)

#### Socket 作为应用层和传输层之间的桥梁，简单通信流程：
![image.png](/images/socketzuoweiyingyongceng.png)

#### TCP的三次握手
![image.png](/images/tcpsanciwoshou.png)

#### TCP的四次挥手
![image.png](/images/tcpsicihuishou.png)

#### 扩展阅读
- https://blog.csdn.net/u011240877/article/details/72863432
- [Android Service完全解析，关于服务你所需知道的一切(上)](http://blog.csdn.net/guolin_blog/article/details/11952435)
- [Android Service完全解析，关于服务你所需知道的一切(下)](http://blog.csdn.net/guolin_blog/article/details/9797169)
