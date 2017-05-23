title: App共享文件Uri不能为file://
date: 2016-06-17  10:30:00
tags:
categories: Android

---


- 先看异常信息:

````
E/StrictMode: null
              java.lang.Throwable: file:// Uri exposed through Intent.getData()
                  at android.os.StrictMode.onFileUriExposed(StrictMode.java:1757)
                  at android.net.Uri.checkFileUriExposed(Uri.java:2346)
                  at android.content.Intent.prepareToLeaveProcess(Intent.java:8045)
                  at android.app.Instrumentation.execStartActivity(Instrumentation.java:1506)
                  at android.app.Activity.startActivityForResult(Activity.java:3930)
                  at android.app.Activity.startActivityForResult(Activity.java:3890)
                  at android.support.v4.app.FragmentActivity.startActivityForResult(FragmentActivity.java:843)
                  at android.app.Activity.startActivity(Activity.java:4213)
                  at android.support.v4.app.ActivityCompatJB.startActivity(ActivityCompatJB.java:26)
                  at android.support.v4.app.ActivityCompat.startActivity(ActivityCompat.java:133)
                  at com.horizon.offer.mail.maildetail.impl.ImageAnnexWrapper.openFile(ImageAnnexWrapper.java:26)
                  at com.horizon.offer.mail.maildetail.MailDetailActivity.openAnnexFile(MailDetailActivity.java:184)
                  at com.horizon.offer.mail.maildetail.adapter.MailAnnexAdapter$MailAnnexViewHolder$1.onClick(MailAnnexAdapter.java:75)
                  at android.view.View.performClick(View.java:5204)
                  at android.view.View$PerformClick.run(View.java:21153)
                  at android.os.Handler.handleCallback(Handler.java:739)
                  at android.os.Handler.dispatchMessage(Handler.java:95)
                  at android.os.Looper.loop(Looper.java:148)
                  at android.app.ActivityThread.main(ActivityThread.java:5417)
                  at java.lang.reflect.Method.invoke(Native Method)
                  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:726)
                  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:616)
````
在StrictMode(严格)模式下，App之间共享资源报的异常。

- 出现这个异常的代码

````
public void openFile(@NonNull Activity activity, @NonNull File file) {
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.addCategory(Intent.CATEGORY_DEFAULT);
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        /**异常这行代码**/
        intent.setDataAndType(Uri.fromFile(file), "image/*");
        ActivityCompat.startActivity(activity, intent, null);
    }
````
打印

````
Uri.fromFile(file)
````
输出信息

````
Uri uri=file:///storage/emulated/0/download/9text.jpg
````
- 各方说明
	- [Android N中说明Uri不能以file://的原因](https://developer.android.com/preview/behavior-changes.html#sharing-files)
		- 传递软件包网域外的 file:// URI 可能给接收器留下无法访问的路径。
		- 访问权限控制 
- [解决办法](https://developer.android.com/preview/behavior-changes.html#sharing-files)
	- [FileProvider](https://developer.android.com/reference/android/support/v4/content/FileProvider.html)
- 总结
	-  开始搜索Uri.fromFile(file)并分析源码
	  
		````
		 /**
     * Creates a Uri from a file. The URI has the form
     * "file://<absolute path>". Encodes path characters with the exception of
     * '/'.
     *
     * <p>Example: "file:///tmp/android.txt"
     *
     * @throws NullPointerException if file is null
     * @return a Uri for the given file
     */
    public static Uri fromFile(File file) {
        if (file == null) {
            throw new NullPointerException("file");
        }

        PathPart path = PathPart.fromDecoded(file.getAbsolutePath());
        return new HierarchicalUri(
                "file", Part.EMPTY, path, Part.NULL, Part.NULL);
    }
		````
		没有找到相关解决方案。
	-  [StrictMode的FileUriExposedException异常检测](https://developer.android.com/reference/android/os/StrictMode.VmPolicy.Builder.html#detectFileUriExposure())中有提到FileProvider的使用
	-  我写的一个[demo](https://github.com/milin411/FileIntentDemo.git)
	-  给以后提醒app之间共享数据传Uri不能传file://
	
-  参考资料
	- [StrictMode](https://developer.android.com/reference/android/os/StrictMode.html) 
	- [Android Uri和Java URI的区别](http://www.jianshu.com/p/5572b42fc63f)
	- [Android的Uri](http://blog.csdn.net/dlutbrucezhang/article/details/8917303) 
	- [Uri详解之——Uri结构与代码提取](http://blog.csdn.net/harvic880925/article/details/44679239)
	- [根据 Android Training课程写的FileProvider小例子](http://blog.csdn.net/zhangyingli/article/details/37902367)
	- [FileUriExposedException 异常](https://developer.android.com/preview/behavior-changes.html#sharing-files)
	- [FileProvider](https://developer.android.com/reference/android/support/v4/content/FileProvider.html)
	- [FileProvider使用](http://blog.csdn.net/zhangyingli/article/details/37902367)
 
