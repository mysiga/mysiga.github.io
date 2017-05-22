title: Android 微信抢红包
date: 2016-01-15  10:30:00
tags:
categories: Android

---


## Android 微信抢红包
- 主要原理：监听微信APP,如果发送[微信红包]相关通知,则通过Android辅助类服务AccessibilityService实现抢红包点击一系列动作

- 通过AccessibilityService服务监听微信
- 主要监听配置如下:

```
监听动作类型
android:accessibilityEventTypes="typeNotificationStateChanged|typeWindowStateChanged|typeViewScrolled"
完成反馈动作类型
android:accessibilityFeedbackType="feedbackGeneric"
标识
android:accessibilityFlags=""
是否可以屏幕内容
android:canRetrieveWindowContent="true"
服务描述
android:description="@string/server_description"
通知反应时间
android:notificationTimeout="0"
监听应用包名
android:packageNames="com.tencent.mm" 
```
		
- AccessibilityService实现方法：
	-  onAccessibilityEvent()时间监听回调方法
	-  onServiceConnected启动服务
	-  onUnbind服务器解绑
	-  onInterrupt中断服务
-  事件
	- AccessibilityEvent.TYPE_NOTIFICATION_STATE_CHANGED:通知消息事件
	-   AccessibilityEvent.TYPE_WINDOW_STATE_CHANGED:屏幕window变化时间
	-   AccessibilityEvent.TYPE_VIEW_SCROLLED屏幕滚动监听事件

- 1.微信后台抢红包：
	- 后台TYPE_NOTIFICATION_STATE_CHANGED事件监听有[微信红包]通知则点击通知，TYPE_WINDOW_STATE_CHANGED事件点击‘抢红包’，TYPE_WINDOW_STATE_CHANGED点击‘拆红包’，TYPE_WINDOW_STATE_CHANGED跳转红包查看详情
- 2.微信当前聊天页面抢红包
	-  当前聊天页面，不能接受通知
	- AccessibilityEvent.TYPE_VIEW_SCROLLED监听屏幕view有新消息，则查看屏幕是否有'抢红包'view，有则点击‘抢红包’view,点击抢红包后则同上通过TYPE_WINDOW_STATE_CHANGED事件完成抢红包一系列动作
- [代码地址](https://github.com/milin411/RedWallet.git)