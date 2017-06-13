title: Java类初始化顺序
date: 2016-06-30 09:29:29
tags:
categories: Java
---

- Print.java
````
public class Print {
 public Print (String print) {
  System.out.println(print);
 }
}
````
- Person.java
````
public class Person {

 private static Print printLog = new Print("父类静态实例1-->");
 private Print log = new Print("父类实例-->");
 static {
  Print printLog = new Print("父类静态块-->");
 }
 private static Print print = new Print("父类静态实例2-->");
 public Person(){
  new Print("父类构造方法-->");
 }
 
 public static void print() {
  Print printLog = new Print("父类静态方法-->");
 }
 private static Print printLog3 = new Print("父类静态实例3-->");
}
````
- Child.java
````
public class Child extends Person {

 private static Print printLog = new Print("子类静态实例1-->");
 private Print log = new Print("子类实例-->");
 static {
  Print printLog = new Print("子类静态块-->");
 }
 private static Print print = new Print("子类静态实例2-->");

 public Child() {
  new Print("子类构造方法-->");
 }

 public static void print() {
  Print printLog = new Print("子类静态方法-->");
 }

 private static Print printLog3 = new Print("子类静态实例3-->");

 public static void main(String[] args) {
  new Child();
 }
}
````
- 输出结果
````
父类静态实例1-->
父类静态块-->
父类静态实例2-->
父类静态实例3-->
子类静态实例1-->
子类静态块-->
子类静态实例2-->
子类静态实例3-->
父类实例-->
父类构造方法-->
子类实例-->
子类构造方法-->
````
->很显然，子类初始化的过程：
  - 父类静态实例或者父类静态代码块，代码的放置先后顺序，决定它们执行先后
 - 子类静态实例或者子类静态代码块
 - 父类实例
 - 父类构造方法
 - 子类实例
 - 子类构造方法
- 那为什么了？后面研究下再跟进