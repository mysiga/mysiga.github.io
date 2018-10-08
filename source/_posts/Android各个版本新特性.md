title: Android各个版本新特性
date: 2018-06-06 14:58:29
tags:
categories: Android
---


目前主流的Android适配版本最低版本4.0或5.0，这里主要总结一下Android 4.0以后各个版本特性，方便适配和面试。加粗标注为面试中问到的重点特性，其他为重要适配特性。（倒序）
- [Android 8.0](https://developer.android.google.cn/about/versions/oreo/android-8.0.html)
  - 优化通知
    - 通知渠道
    - 通知标志
    - 休眠
    - 通知超时
    - 通知设置
    - 通知清除
  - 自动填充框架
  - 画中画模式：清单中Activity设置android:supportsPictureInPicture
  - **可下载字体**：[FontRequest](https://developer.android.google.cn/reference/android/provider/FontsContract.html)
  - [XML 中的字体](https://developer.android.google.cn/guide/topics/ui/look-and-feel/fonts-in-xml.html)
  - **自动调整 TextView 的大小**
  - [自适应图标](https://developer.android.google.cn/guide/practices/ui_guidelines/icon_design_adaptive.html)
  - 颜色管理
  - [WebView API](https://developer.android.google.cn/guide/webapps/managing-webview.html)
  - 多显示器支持
  - **统一的布局外边距和内边距**
![image.png](http://upload-images.jianshu.io/upload_images/1534431-b4ba4d688a817290.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 指针捕获
  - 应用类别
  - Android TV 启动器
  - AnimatorSet
  - **新的 StrictMode 检测程序**
![image.png](http://upload-images.jianshu.io/upload_images/1534431-e33af7003dde9550.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 缓存数据
  - **findViewById() 签名变更**
![image.png](http://upload-images.jianshu.io/upload_images/1534431-89c0141aa910db0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - **权限**
![image.png](http://upload-images.jianshu.io/upload_images/1534431-f4c146ef0be3c03f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - **更新的 Java 支持**
![image.png](http://upload-images.jianshu.io/upload_images/1534431-0464c897b3051935.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- [Android 7.1](https://developer.android.google.cn/about/versions/nougat/android-7.1.html)
	- 加入重启按钮
	- App圆形图标
	- 添加新的Emoji
- [Android 7.0](https://developer.android.google.cn/about/versions/nougat/android-7.0-changes.html)
	- 电池和内存
		- 低电耗模式
		- Project Svelte：后台优化
	- **权限更改**
		- 系统权限更改
	- **在应用间文件共享权限控制**
	- **多窗口支持**
	- **通知栏快捷回复**
	- **支持VR**
	- 引入JIT编译器
	- **画中画**
	- **App快捷菜单**
- [Android 6.0](https://developer.android.google.cn/about/versions/marshmallow/android-6.0-changes.html)
	- **运行时请求权限**
	- 低电耗模式和应用待机模式
	- **取消支持 Apache HTTP 客户端**
	- BoringSSL
	- 硬件标识符访问权
	- 通知
	- 音频管理器变更
	- **支持文本选择**
	- Android 密钥库不再支持 DSA。但仍支持 ECDSA
	- WLAN 和网络连接变更
	- 相机服务变更
	- APK 验证
	- USB 连接
- [Android 5.0](https://developer.android.google.cn/about/versions/android-5.0-changes.html#managed_profiles)
  - **Android Runtime (ART)默认运行平台设置**
  - **通知**
  		- Material Design 样式
  		- 声音和振动
  		- 锁定屏幕可见性
  		- 媒体播放
	  	- 浮动通知
  - **引入Material Design设计**
  - 支持OpenGL ES3.1 
  - 媒体控件和 RemoteControlClient
  - getRecentTasks()
  - **支持Android NDK中的64位**
  - **只能显示绑定到服务，取消隐藏绑定服务**
  - WebView API修改
  - 自定义权限唯一性要求
  - **TLS/SSL 默认配置变更**
	  	- 服务器不支持任何已启用的加密套件
	  	- 应用对用于连接服务器的加密套件做出错误的假设
	  	- 服务器不支持 TLSv1.1、TLSv1.2 或新的 TLS 扩展
  - 支持托管配置文件
- [Android 4.4](https://developer.android.google.cn/about/versions/kitkat.html)
	- **支持Android Beam**
	- **添加打印框架**
	- 存储访问框架
	- 低功耗传感器
	- 添加短信提供程序
	- **添加全屏沉浸模式**
        - [Android 沉浸式状态栏的三种实现方式](http://www.jianshu.com/p/be2b7be418d7#)
        - [兼容库SystemBarTint](https://github.com/jgilfelt/SystemBarTint)
	- **添加透明系统 UI 样式**
	- 添加新的媒体功能
	- RenderScript Compute
		- 持续性能提升
		- **GPU 加速**
		- Android NDK 中的 RenderScript
	- 图形
		- GLES2.0 SurfaceFlinge
		- 新的硬件合成器支持虚拟显示
	- 支持新的连接类型
		- 新的蓝牙配置文件
		- 红外发射器
		- Wi-Fi TDLS 支持
	- 无障碍功能
	- 安全增强功能
	- 内存使用率分析工具
		- Procstats 
- [Android 4.0,4.1,4.2,4.3](https://developer.android.google.cn/about/versions/jelly-bean.html)
	- 支持OpenGL ES 3.0
	- 增强蓝牙连接
	- 优化位置和传感器
	- **添加转场动画**
	- 支持Daydream
	- **人脸识别解锁**
	- **Photo Sphere 全景相片**


	


