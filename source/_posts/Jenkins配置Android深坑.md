title: Jenkins配置Android深坑
date: 2017-07-17 09:47:29
tags:
categories: 其他
---


在Jenkins上配置Android的一些持续集成（自动打包上传，单元测试，集成测试等）应该是很普遍的事情了。Jenkins一般配置在一个通用的测试服务器上，原来都是拿一个mac当测试服务器，在上面想怎么玩怎么就怎么玩，想怎么配置就怎么配置。可是如果测试服务器是在虚拟机上而且测试服务器不是你能控制的，那就要在Jenkins上配置Android就不能想自己控制自己电脑那边方便了，在配置过程中也填了不少的坑。那就讲下填坑之路吧。

#### Jenkins安装
 这部分一般不用担心，在公司里会有专门的人员安装，你只管用就可以了。那如果是由你负责安装Jenkins的话那就要好好看Jenkins官网的安装方法了，[根据不同的平台下载不同的安装包安装](https://jenkins.io/download/)
 
#### Jenkins上新建项目
在打开的Jenkins页面左上方找到"创建"(create),
![image.png](http://upload-images.jianshu.io/upload_images/1534431-cbd759e33b51c05c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，写项目名和项目类型

![image.png](http://upload-images.jianshu.io/upload_images/1534431-031ffec2a56338d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

(因为是Android项目，默认就选择第一个。)
然后选择"OK"
然后,一个新建的项目就好了。

![image.png](http://upload-images.jianshu.io/upload_images/1534431-f3da863f34edd75e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### Jenkins配置项目
配置前提条件就是你[jenkins要安装好几个插件](https://jenkins.io/doc/book/managing/plugins/): git插件，gradle插件，Android虚拟机插件
![image.png](http://upload-images.jianshu.io/upload_images/1534431-d563fd7f6289c76a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/1534431-630008664ec0e4df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/1534431-096e22f8f71b8d9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后就开始
[Jenkins官网配置Androd环境](https://www.zuehlke.com/blog/en/configure-your-android-project-on-jenkins/)，
- 选择JDK

![image.png](http://upload-images.jianshu.io/upload_images/1534431-8dc9b1da00eb9751.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 源码

![image.png](http://upload-images.jianshu.io/upload_images/1534431-5d5da1a0efcb3f73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 构建环境

![image.png](http://upload-images.jianshu.io/upload_images/1534431-0652ce7865dadf45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)虚拟机配置后期关闭，配置虚拟机主要用来下载andoid-sdk。

- 构建

![image.png](http://upload-images.jianshu.io/upload_images/1534431-a430e352881caaaa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OK,好了，这就是正确的配置，那我们就运行下开始深坑之旅了。

![image.png](http://upload-images.jianshu.io/upload_images/1534431-f199552ec5263663.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看下输入log

![image.png](http://upload-images.jianshu.io/upload_images/1534431-dcdc202bcd339294.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####问题不分先后，按解决问题处理
- 问题1

![image.png](http://upload-images.jianshu.io/upload_images/1534431-fe1cf269790bc480.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虚拟机没有找到相应的hardware配置，尝试更改各种配置方法最后都是失败结束（如果你有其他解决方法记得告诉我）。能走到这一步就说明你的andoid-sdk已经按照好了，只是打开虚拟机还有问题。想想本身主要用来gradle打包Android apk，还用不到打开虚拟机，能安装好Android-sdk就可以。然后在项目配置中![Alt text]
![image.png](http://upload-images.jianshu.io/upload_images/1534431-1dee90179b1159f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

勾选掉虚拟机配置就可以了，不影响你gradle打包。

- 问题2

![image.png](http://upload-images.jianshu.io/upload_images/1534431-459d738485a45673.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Android环境变量没有配置，需要根据Android虚拟机配置下载来的android sdk位置配了，

![image.png](http://upload-images.jianshu.io/upload_images/1534431-5c08d301ca0b3962.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

仔细看下android虚拟机下载sdk的位置，然后在jenkins的系统配置中配置相应的环境变量
![image.png](http://upload-images.jianshu.io/upload_images/1534431-d1dcc03ef8d1b69c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就可以了。

- 问题3

![image.png](http://upload-images.jianshu.io/upload_images/1534431-5e890bdc058bdc5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虚拟机的ABIs没有配置好,需要项目配置

![image.png](http://upload-images.jianshu.io/upload_images/1534431-067d2f34dbe6ae82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就OK了。

- 问题4
![image.png](http://upload-images.jianshu.io/upload_images/1534431-42d5c68603b71397.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这个是说你下载的android-sdk需要同意协议，可是我们这个是虚拟的服务器上没有办法点击同意啊，那就看提示解决方法地址

![image.png](http://upload-images.jianshu.io/upload_images/1534431-385369d144163db1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1534431-3d0e78dbf20fe800.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有两种解决方案，一种在ui上点，一种是把你已经同意好的协议文件拷贝到当前页面。那我们只能把自己电脑的电脑拷贝进去了，可是又遇到问题，我没有权限访问这个服务器那怎么复制到这个jekins服务器上了，那只能把这个协议文件上传到[七牛](https://www.qiniu.com/)（或者其他托管服务器上），脚本如下
```
# 进入下载好的android-sdk路径
cd /root/.jenkins/tools/android-sdk
# 新建协议文件夹
mkdir licenses
# 进入协议文件夹内
cd /root/.jenkins/tools/android-sdk/licenses
# rm -rf android-sdk-license
# 下载已经同意好的android协议文件
wget http://7xn0ue.com1.z0.glb.clouddn.com/android-sdk-license
```
具体项目配置

![image.png](http://upload-images.jianshu.io/upload_images/1534431-da840093cfec03e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OK,重新构建一下，不过记得先把“构建环境”的配置勾选掉，要不然“构建环境有问题执行不到这个脚本”

- 问题5

![image.png](http://upload-images.jianshu.io/upload_images/1534431-82183912bc907638.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你运行的系统缺少libGL.so.1库，那就需要安装这个库，找了下可以用
```
yum install mesa-libGL -y
```脚本解决,已经要记得加“-y ”要不然会出错提示

![image.png](http://upload-images.jianshu.io/upload_images/1534431-23a8a172c18ff124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 问题6

![image.png](http://upload-images.jianshu.io/upload_images/1534431-570f151c6e7b1551.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你运行的系统内核版本过低需要升级，这个也是我比较纠结的要是升级那会不会影响测试服务器的运行。（如果你现在是正式环境下你就需要跟领导沟通只是斟酌了）。
通过以下脚本可以解决，同
```
# 下载glibc-2.15.tar.gz内核文件，根据不同版本在http://ftp.gnu.org/gnu/glibc/
下载
wget http://ftp.gnu.org/gnu/glibc/glibc-2.15.tar.gz
# 解压文件
tar -xvf glibc-2.15.tar.gz
# 依赖库(glibc-ports-2.15.tar.gz)
wget http://ftp.gnu.org/gnu/glibc/glibc-ports-2.15.tar.gz
# 解压
tar -xvf glibc-ports-2.15.tar.gz
# 依赖库解压目录移到到主目录中
mv glibc-ports-2.15 glibc-2.15/ports
# 创建编译目录
mkdir glibc-build
cd glibc-build
# 运行以下命令编译及安装
../glibc-2.15/configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
make
make install
```
配置如下
![image.png](http://upload-images.jianshu.io/upload_images/1534431-f5153e0d9295fde4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



以上是我遇到的所有问题，希望这些坑可以对你有帮助。
那看下最后成功输出log和配置：

![log](http://upload-images.jianshu.io/upload_images/1534431-5a7844d2a0ded788.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![JDK](http://upload-images.jianshu.io/upload_images/1534431-1e5e1234553f21e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![源码仓库](http://upload-images.jianshu.io/upload_images/1534431-adb38281522b4c42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![构建触发器和构建环境](http://upload-images.jianshu.io/upload_images/1534431-f59eff5b1878425c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![构建](http://upload-images.jianshu.io/upload_images/1534431-7705c3fbf55d287f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)