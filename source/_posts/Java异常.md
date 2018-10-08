title: Java异常
date: 2018-07-16  18:30:00
tags:
categories: Java
---



## Java异常

![Java异常](https://upload-images.jianshu.io/upload_images/1534431-11903c56d9e318ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 对比Exception和Error 
- 运行异常与一般异常区别
- ClassNotFoundException和NoClassDefFoundError有什么区别
- throw和throws
- 异常处理几点建议
- 自定义异常
###对比Exception和Error。
![Exception和Error](https://upload-images.jianshu.io/upload_images/1534431-ad557fa71b48a3ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Exception和Error都继承Throwable,
- Exception是程序可以处理的异常，正常JVM运行外的异常。
- Error一般是JVM相关异常错误，不可恢复状态，也不方便捕获。
###运行异常与一般异常区别
Eeception又分为可检测异常（Checked Exception）和运行异常（也可叫不可检查异常，Runtime Exception）。
- 可检查异常（Checked Exception）在代码里必须显式地进行捕获处理，可以被处理的异常。如：IOException。
- 运行异常,类似NullPointerException,ArrayIndexOutOfBoundsException,通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译器强制要求。
###ClassNotFoundException和NoClassDefFoundError有什么区别
- ClassNotFoundException:Java动态加载类的时候，如果这个类在类路径中没有被找到或者当一个类已经某个类加载器加载到内存中了，此时另一个类加载器又尝试着动态地从同一个包中加载这个类就会抛出ClassNotFoundException异常。类加载中加载时从外存储器找不到需要的class。
- NoClassDefFoudError，指要查找的类在编译的时候是存在的，运行的时候却找不到的异常错误。类加载中连接时从内存找不到需要的class。
###throw和throws
```
public void custormException()throws  CustomException{
throw new CustomException("抛出CustormException");
}
```
- throw作用域方法体内
- throws方法名后面
###异常处理几点建议
- 只针对不正常的情况才使用异常,不需要异常处理的不要专门处理以增加系统额外检查降低性能。
```
try {
int i=0;
while (true) {
arr[i]=0;
i++;
}
} catch (IndexOutOfBoundsException e) {
}
```
- 抛出的异常要适合于相应的抽象
```
public E get(int index) {
try {
return listIterator(index).next();
} catch (NoSuchElementException exc) {
throw new IndexOutOfBoundsException("Index: "+index);
}
}
```
- 不要忽略异常，避免使用空的catch语句
```
try {
...
} catch (SomeException e) {
}
```
- 异常保证输出堆栈信息，在异常消息中加入导致异常发生的全部信息
```
try {
...
} catch (SomeException e) {
System.out.println("12344");
}
```
###自定义异常
一般根据项目需要自定义异常，特别是服务端处理业务的时候。一般自定义异常继承Exception，Throwable（因为Error一般用于JVM的异常错误，不常用自定义），因为异常已实现相关方法，只需重写构造方法就可以。代码如下：
-  继承Exception
```
public class CustomException extends Exception {
public CustomException() {
}

public CustomException(String message) {
super(message);
}

public CustomException(String message, Throwable cause) {
super(message, cause);
}

public CustomException(Throwable cause) {
super(cause);
}

public CustomException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
super(message, cause, enableSuppression, writableStackTrace);
}
}
```
```
public class TestException {
public void testMethod(){
try {
custormException();
} catch (CustomException e) {
e.printStackTrace();
}
}
public void custormException()throws  CustomException{
throw new CustomException("抛出CustormException");
}
public static void main(String[] args) {
TestException testException=new TestException();
testException.testMethod();
}
}
```
输出
```
com.mysiga.learn.throwable.CustomException: 抛出CustormException
at com.mysiga.learn.throwable.TestException.custormException(TestException.java:15)
at com.mysiga.learn.throwable.TestException.testMethod(TestException.java:9)
at com.mysiga.learn.throwable.TestException.main(TestException.java:19)

```
-  继承Throwable
```
public class CustomThrowable extends Throwable {
public CustomThrowable() {
}

public CustomThrowable(String message) {
super(message);
}

public CustomThrowable(String message, Throwable cause) {
super(message, cause);
}

public CustomThrowable(Throwable cause) {
super(cause);
}

public CustomThrowable(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
super(message, cause, enableSuppression, writableStackTrace);
}
}
```
```
public class TestThrowable {
public void testMethod(){
try {
custormException();
} catch (CustomThrowable e) {
e.printStackTrace();
}
}
public void custormException()throws  CustomThrowable{
throw new CustomThrowable("抛出CustomThrowable");
}
public static void main(String[] args) {
TestThrowable testThrowable=new TestThrowable();
testThrowable.testMethod();
}
}
```
输出
```
com.mysiga.learn.throwable.CustomThrowable: 抛出CustomThrowable
at com.mysiga.learn.throwable.TestThrowable.custormException(TestThrowable.java:15)
at com.mysiga.learn.throwable.TestThrowable.testMethod(TestThrowable.java:9)
at com.mysiga.learn.throwable.TestThrowable.main(TestThrowable.java:19)

```
