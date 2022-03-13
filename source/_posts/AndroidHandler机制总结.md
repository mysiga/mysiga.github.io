title: Android Handler机制总结
date: 2021-09-03  19:20:00
tags:
categories: Android

---



#### Handle机制
- APP的启动过程中调用ActivityThread类main方法初始化MainLooper
- Handler创建Message并发送给Looper
![image.png](/images/handler1.png)
- Looper循环处理MessageQueue的Message
![image.png](/images/handler2.png)
最后message通过target找到发送消息相对应的handle进行回调处理。
- 子线程创建handle
```
Thread {
                //初始化线程looper对象
                Looper.prepare()
                //初始化子线程handler,handler关联子线程looper
                threadHandler =
                    Handler(
                        Looper.myLooper()!!,
                        Handler.Callback {
                            Log.i("milin", "threadHandler:${it.what}")
                            return@Callback true
                        })
                //调用此方法，消息才会循环处理
                Looper.loop();
                //发送消息
                threadHandler?.sendEmptyMessage(123)
            }.start()
```

- 主线程Looper.loop()一直死循环，为什么没有卡死？
主要死循环中Looper.loop()中
```
Message msg = queue.next(); // might block
```
没有消息Linux底层一直在等待，不占用CPU。
![image.png](/images/nativePollllOnce.png)
具体看[nativePollOnce函数分析](https://www.kancloud.cn/alex_wsc/android-deep2/413394)

#### 文章
- [Android Handler 机制（一）：Handler 运行机制完整梳理](https://www.cnblogs.com/renhui/p/12857876.html)
- [Android Handler 机制（二）：Handler 机制深入探究问题梳理](https://www.cnblogs.com/renhui/p/12852347.html)
