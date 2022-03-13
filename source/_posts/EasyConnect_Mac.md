title: 初始化失败,请尝试重新安装
date: 2020-11-25 00:19:09
tags:
categories: 其他
---


#### EasyConnect mac “初始化失败,请尝试重新安装”
一、 问题
使用EasyConnect for mac的用户是不是会经常出现这样的提示：“初始化失败，请尝试重新安装”![初始化失败,请重新安装](/images/chushihuashibai_qingchangshichongxinanzhuang.png)
前端时间也碰到这个坑，点击重新下载安装后还一直提示这个，折腾了好几天最终在网上找到这个用户答案才解决：https://blog.csdn.net/langzichai/article/details/109488191
出现这个问题最主要原因是EasyConnect启动的时候会默认开启两个守护进程，如果你把他们两个进程禁止启动的，EasyConnect就无法初始化。![守护进程](/images/shouhujingchengxiang.png)

二、解决方案
知道问题的根因，那我们解决方法就是我们把这两个进程不禁止启动就可以了，要解决这个还需要用到腾讯的工具,[下载地址](https://lemon.qq.com)
下载安装完后，打开用户界面，点击“开机启动项管理”，找到这两个进程打开启动，然后重新重启下电脑就好了。
![image.png](/images/qidongxiang.png)
![image.png](/images/kaijiqidongxiangguanli.png)
![image.png](/images/kaijiqidongxiang_qidong.png)

三、为什么会禁止掉了？
你肯定会问为什么会禁止掉了？这个我也不清楚，从很多朋友反馈出现的问题原因是
- 使用第三方软件，如：lemon，clean for mac 等清除垃圾软件会导致这样的问题。
- 升级mac os系统默认关闭了这两个进程

我的总结就在这里， 你问题解决了吗？我现在用的是EasyConnect7.6.3，如果没有解决记得给我留言哦。

