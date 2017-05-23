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
 
## 谓词 - 刘康

- NSPredicate是Foundation框架中的一个类。
- 作用：指定数据被获取和过滤的方式。提供了类似于自然语言一样定义一个集合被搜寻的逻辑条件。

为了证明NSPredicate的强大功能，先写一个Person类，做准备工作。

```
class Person: NSObject {
    var name: String
    var age: Int
    init(name: String, age: Int) {
        self.name = name
        self.age = age
        super.init()
    }
    override var description: String {
        return "name: \(self.name), age: \(self.age)"
    }
}

var colleagues = NSMutableArray()
colleagues.addObject(Person(name: "Arthur", age: 45))
colleagues.addObject(Person(name: "Michael", age: 23))
colleagues.addObject(Person(name: "Kenny", age: 25))
colleagues.addObject(Person(name: "Bella", age: 24))
colleagues.addObject(Person(name: "Vincent", age: 36))
colleagues.addObject(Person(name: "Adolph", age: 39))
```
###基本使用

**找出年龄为24岁的人:**

```
let predicateByAge = NSPredicate(format: "age == 24")
let result = colleagues.filteredArrayUsingPredicate(predicateByAge)
```

**参数可以传入:**

```
let age = NSNumber(int: 25)
let predicateByPassAge = NSPredicate(format: "age == %@", age)
let result1 = colleagues.filteredArrayUsingPredicate(predicateByPassAge)
```

**也可传入要对比的属性，这里是age. 属性的key:**

```
let pridicateByAge1 = NSPredicate(format: "%K == %@", "age", NSNumber(int: 36))
let result2 = colleagues.filteredArrayUsingPredicate(pridicateByAge1)
```

**指定通配的变量，这里用24来替代age:**

```
let pridicateByAge2 = NSPredicate (format: "age == $age")
let result3 = colleagues.filteredArrayUsingPredicate(pridicateByAge2.predicateWithSubstitutionVariables(["age": NSNumber(int: 24)]))
```

####语法小结
 - 使用`%@`对应数字，字符串，日期的替代值
 - 使用`%K`对应要比较的属性，也就是KVC中的key
 - 使用$变量名来表示通配的变量，然后`predicateWithSubstitutionVariables`来决定具体的变量值

###基本比较

**找出年龄大于40岁的同事:**

```
let predicateAgeOver40 = NSPredicate(format: "age > 40")
let boss = colleagues.filteredArrayUsingPredicate(predicateAgeOver40)
```

**找出年龄在22岁~35岁的人:**

```
let minAge = NSNumber(int: 22)
let maxAge = NSNumber(int: 35)
let predicateByAge3 = NSPredicate(format: "age BETWEEN {%@, %@}", minAge, maxAge)
let result4 = colleagues.filteredArrayUsingPredicate(predicateByAge3)
```

####语法小结
 - `>` 大于
 - `>=` 大于等于
 - `<` 小于
 - `<=` 小于等于
 - `==` 等于
 - `!=` 或者 `<>` 不等于
 - `BETWEEN` 介于两者之间,包括上下限


###复合比较
 - `&&` 或者`AND` 逻辑与
 - `||` 或者 `OR` 逻辑或
 - `!`或者`NOT` 逻辑非

```
let predicateByCompare = NSPredicate(format: "age < 30 OR age >= 40")
let result5 = colleagues.filteredArrayUsingPredicate(predicateByCompare)
```

###字符串比较

 - `BEGINSWITH` 左边表达式以右边表达式开头
 - `CONTAINS` 左边表达式包含右边表达式
 - `ENDSWITH` 左边表达式以右边表达式结尾
 - `LIKE` 左边表达式和右边表达式相似（简单的正则表达式匹配，?匹配一个字符，*匹配0个或者多个字符）
 - `MATCHES` 可以实现较为复杂的正则表达式匹配
 - 用方括号加`cd`来不区分大小写和变音符号
 - `IN` 左边的表达式在右边的集合里

**找出名字以“A”开头的同事:**

```
let pridivateByName1 = NSPredicate(format: "name BEGINSWITH %@", "A")
let result6 = colleagues.filteredArrayUsingPredicate(pridivateByName1)
```

**名字里包含in,不区分大小写，并且年龄大于等于24:**

```
let pridivateByName2 = NSPredicate(format: "name CONTAINS %@ && age >= %@", "in", NSNumber(int: 24))
let result7 = colleagues.filteredArrayUsingPredicate(pridivateByName2)
```

**复合正则表达式`T[a-z]*k`:**

```
let privatedivateByName3 = NSPredicate(format: "name MATCHES 'T[a-z]*k'")
let result8 = colleagues.filteredArrayUsingPredicate(privatedivateByName3)
```

**名字是两者中的一个:**

```
let privatedivateByName4 = NSPredicate(format: "name IN {'Bella', 'Jack Tomphon'}")
let result9 = colleagues.filteredArrayUsingPredicate(privatedivateByName4)
```

###基于Block的谓词

**基于Block能够灵活的定制谓词，这里简单的Block定义`age > 24`:**

```
let blockPredicate = NSPredicate { (person: AnyObject!, _) -> Bool in
    var result = false
    if let castResult = person as? Person {
        if castResult.age > 24 {
            result = true
        }
    }
    return result
}
let result10 = colleagues.filteredArrayUsingPredicate(blockPredicate)
```

###Array使用谓词参考：
[StackOverFlow](http://stackoverflow.com/questions/24382580/filteredarrayusingpredicate-does-not-exist-in-swift-array)

