title: Java单例模式，线程安全
date: 2016-05-28 14:50:29
tags:
categories: Java
---

- 懒汉式:线程安全，开销大

````
public class Singleton{
 private static final Singleton singleton;
 public static final synchronized Singleton getInstance(){
 	if(singleton==null){
 	 singleton=new Singleton();
 	}
 	return singleton;
 }
}
````
- 双重检查锁：线程安全，根据需求使用

````
public class Singleton{
  private volatile static Singleton singleton;
  public static final Singleton getInstance(){
  	if(singleton==null){
  	  synchronized(Singleton.class){
  	    if(singleton==null){
  	    singleton=new Singleton();
  	    }
  	  }
  	}
   return singleton;
  }
}

````
- 静态内部类锁：线程安全，比较推荐

````
public class Singleton{
private static class Holder{
public static final Singleton singleton=new Singleton();
}
public static final Single getInstance(){
return Holder.singleton;
}
}
````
- 饿汗式：线程安全，但没有实现懒加载

````
public class Singleton{
 private static final Singleton singleton=new Singleton();
 public static final Singleton getInstance(){
 return singleton;
 }
}
````
- 枚举:线程安全，但比较少人用

````
public class Singleton{
public enum EasySingleton{
    INSTANCE;
}
}
````

- 参考文章
	- [具体JVM机制](http://www.infoq.com/cn/articles/double-checked-locking-with-delay-initialization)
	- [具体代码讲解](http://wuchong.me/blog/2014/08/28/how-to-correctly-write-singleton-pattern/)