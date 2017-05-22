title: Moto 360 Sport初体验
date: 2016-03-04  10:30:00
tags:
categories: Android

---


## Moto 360 Sport初体验

- 前奏
	- 美版
	- 第一连接需要连接Android手机
	- 需要翻墙
- 应用
	- Android  Wear
	- WearADay
- iPhone支持
	- 无需越狱，连接应用WearADay
		- ![如图1](http://pic.5577.com/up/2015-5/2015050417055266573.jpg)
		- ![如图2](http://pic.5577.com/up/2015-5/2015050417055336913.jpg)
		- ![如图3](http://pic.5577.com/up/2015-5/2015050417055318883.jpg)
		- ![如图4](http://pic.5577.com/up/2015-5/2015050417055318092.jpg)
		- ![如图5](http://pic.5577.com/up/2015-5/2015050417055331041.jpg)
	- 包括来电，信息以及各种应用的通知，包括 QQ，微信等等在 iPhone 和 Moto360 配对成功之后都可以在 Moto360 上显示
		- ![如图1](http://pic.5577.com/up/2015-5/2015050417055428219.jpg)
		- ![如图2](http://pic.5577.com/up/2015-5/2015050417055481895.jpg)
		- ![如图3](http://pic.5577.com/up/2015-5/2015050417055447453.jpg)
		- ![如图4](http://pic.5577.com/up/2015-5/2015050417055512184.jpg)
		- ![如图5](http://pic.5577.com/up/2015-5/2015050417055547831.jpg)
		- ![如图6](http://pic.5577.com/up/2015-5/2015050417055580691.jpg)
- Android支持
	-  微信:表情回复，语音回复，文字回复
	-  QQ:未测
- 缺陷
	- 分辨率不高
	- 目前测试没有好的输入法每次提示都是在连接手机输入但每次都是提示手机不在附近
	

- 开发者模式安装应用
	1. 打开手机以及手表的开发者选项
		- 手机端：设置-关于手机，找到“版本号”，对着它连续点击7下即可开启，开机后进入“设置”-“开发者选项”，勾选“USB调试
		- ![手机开启develp](http://image.anruan.com/imglist/20141021/20141021863912.png)
		- 手表端：长按右侧按钮唤出设置界面-“About”，找到“Build number”，对着它连续点击7下即可开启，开启后进入设置-“Developer options”，按图所示开启“ADB debugging”和“Debug over Bluetooth”
		- ![Moto开启develp](http://image.anruan.com/imglist/20141021/20141021863360.png)
	2. 打开Android Wear软件，勾选设置菜单里的“Debugging over Bluetooth”。
		- 你可以发现“Host”是disconnected未连接状态，这是代表手表端;“Target”是手机端，connected已连接状态;怎么让手表端的显示已连接呢?请继续往下看
		- ![Android Wear打开蓝牙调试](http://image.anruan.com/imglist/20141021/20141021863594.png) 
	3. 把手机连接上电脑，在adb工具的文件夹里调出命令行，依次输入以下命令
	
	````
	adb forward tcp:4444 localabstract:/adb-hub
　　adb connect localhost:4444
	````
	分两次输入(电脑需支持adb命令，如果不支持请百度[Mac配置adb](http://blog.sina.com.cn/s/blog_6268f10201016s64.html)，[Window配置adb](http://blog.csdn.net/huangbiao86/article/details/6664779)两个链接是未测试)
	
	- 输完以上两条命令后，留意手机，会弹出允许USB调试的窗口，勾选总是允许，点击OK即可，这时你就发现代表手表端的“Host”就变成connected已连接状态了，嗯，这就代表可以再电脑执行adb命令来控制手表了
	- ![提示是否调试](http://image.anruan.com/imglist/20141021/20141021863363.png)
	4. 相关命令
		- adb devices (显示已连接的设备，如图所示，会显示出2个设备名，分别对应手机和手表)
		- ![如图](http://image.anruan.com/imglist/20141021/20141021863452.png)
		
		- adb -s 手机ID号 install xxx.apk(给手机安装软件，要注意加上手机ID号，比如我的就是 adb -s TA26901ZLK install xxx.apk )
		- adb -s localhost:4444 install xxx.apk(给手表安装软件，MOTO 360的软件apk可以通过Android Wear软件专题下载，这里是通过手机的蓝牙给手表安装的，所以传输速度稍慢，要耐心等待，安装成功后会有提示)
		- adb -s localhost:4444 unistall xxx(卸载手表端的某个软件，比如adb -s localhost:4444unistall com.whirlscape.minuum)
		- 查看Moto 360所有app的packages
		
		````
		adb -s localhost:4444 shell pm list packages
		````
		- 卸载指定包名的app
		
		````
		adb -s localhost:4444 uninstall [-k] <insert package name here>
		````
		
- 参考文章
	- [链接1](http://m.anruan.com/view_5488.html)
	- [链接2](https://www.reddit.com/r/moto360/comments/3bkd5r/remove_wear_apps_without_uninstalling_app_from/) 
	- [链接3](http://www.5577.com/wear/58563.html)
 


