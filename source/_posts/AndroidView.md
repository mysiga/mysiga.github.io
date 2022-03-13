title: Android View
date: 2021-09-08  10:20:00
tags:
categories: Android

---



- [Window、View、Activity](https://mp.weixin.qq.com/s?__biz=MzU4NDc1MjI4Mw==&mid=2247484180&idx=1&sn=080cccd32f0c58dd68f743b3c78a7e1f&chksm=fd944ee0cae3c7f67f20b82e7a505808fc424a34c737bd0c944478ee7af1162cd98e78d716f2&scene=21#wechat_redirect)
![image.png](/images/activityphonewindowdecorview.png)

- [Android View绘制](https://cloud.tencent.com/developer/article/1601353)
![View绘制](/images/viewhuizhi.png)
- [Android中慎用View#getViewTreeObserver#addOnGlobalLayoutListener来获取view的高度](https://juejin.cn/post/6844903858511020039)
- [源码解析View.post()](https://www.cnblogs.com/dasusu/p/8047172.html)
```
Activity在onCreate（）执行View.post(Runnable)，缓存Runnable
ViewGroup和View执行完ViewRootImpl.performTraversals（measure，layout，draw）后执行Runnable回调
```
- [Android自定义View](https://www.jianshu.com/p/705a6cb6bfee?utm_campaign=haruki&utm_content=note&utm_medium=reader_share&utm_source=weixin)
![自定义View](/images/zidingyiview.png)

- invalidate()与postInvalidate(),requestLayout()三者区别？

 | invalidate() |  postInvalidate()  |  requestLayout()  | 
 | :----- | :----- |  :----- | 
  | 在ui线程执行 | 在工作线程执行  |   --  | 
  |  onDraw()  |  onDraw()  |   onMeause()，onLayout(),onDraw() | 
- onMeasure()有几种Mode?
> 有三种模式：
　UNSPECIFIED
　　这说明parent没有对child强加任何限制，child可以是它想要的任何尺寸。
　EXACTLY
　　Parent为child决定了一个绝对尺寸，child将会被赋予这些边界限制，不管child自己想要多大。
　AT_MOST
　　Child可以是自己任意的大小，但是有个绝对尺寸的上限。
- [View生命周期](https://www.jianshu.com/p/88054f481f17)
 
- View常用方法
- onAttachedToWindow,onDetachedFromWindow
- RecyclerView,ListView
- ViewPager
- [Android 界面指南](https://developer.android.google.cn/guide/topics/ui)
- [软件绘制和硬件绘制区别]（https://developer.android.google.cn/guide/topics/graphics/hardware-accel#software-model）
软件的绘制过程
```
软件的绘制过程
1、对层次结构进行无效化处理
2、绘制层次结构
```
缺点：
```
1、未视图改变的也会重新绘制  
2、存在重复绘制同一个视图
```
硬件加速绘制
```
1、对层次结构进行无效化处理
2、记录并更新显示列表（）
3、绘制显示列表
```
- [surfaceview vs textureview](https://source.android.com/devices/graphics/arch-tv?hl=zh-cn)