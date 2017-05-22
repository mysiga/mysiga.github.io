title: Android emoji表情分享
date: 2015-05-15 23:04:29
tags:
categories: Android
---


## Android emoji表情分享

- 什么是emoji表情？

	- 一套起源于日本的12x12像素表情符号，由栗田穣崇（Shigetaka Kurit）创作，最早在日本网络及手机用户中流行.

- emoji表情手机的支持？
    - IOS
		- iOS 4以及之前版本, 采用Softbank编码.
		- iOS 5以及之后的版本，或者OSX Lion之后的系统, 则改为使用了Unicode6.0编码

    - Android
	    - 在android4.4开始支持emoji表情，Unicode编码,如sougou，谷歌等输入法都支持emoji表情。


- emoji展示过程

	- 显示emoji表情----选中emoji表情----unicod编码字符串----ios或者android对unicode编码字符串处理-----如果是emoji表情编码---则从系统中显示相应的emoji表情图片。

- emoji表情的存储

    - iPhone：统一用unicode6.0编码保存

	- android或wp其他手机： 如果没有emoji表情库，将无法输入。针对输入问题,将统一采用unicode6.0编码存储，UBB代码.

    - 数据库存储：存数据以UTF-8编码用3个字节去存储的，而emoji表情要用4个字节的utf8，也就是utf8mb4格式.
        - 数据库编码转为utf8mb4,
 		- emoji表情转为支持的utf-8，如转为UBB代码([emoji]2600[/emoji])，HTML转义字符(&#x2600)
- emoji表情不支持处理
    - [链接地址1](http://ragnraok.github.io/android-emoji-font-method.html)
    - [链接地址2](http://bbs.csdn.net/topics/390055415)

