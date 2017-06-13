title: Excption与Error包结构
date: 2016-07-31 14:50:29
tags:
categories: Java
---


- [原文来自](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.md)
- [Excption与Error包结构。OOM你遇到过哪些情况，SOF你遇到过哪些情况](http://www.cnblogs.com/yumo/p/4909617.html)
	- Java异常架构图
	![Java异常架构图](https://camo.githubusercontent.com/2248cdf95b8f2569ee0e58aac3d8ed179c89f190/687474703a2f2f696d61676573323031352e636e626c6f67732e636f6d2f626c6f672f3637393930342f3230313531302f3637393930342d32303135313032353231303831333938392d3932313932373931362e6a7067) 
	
	1. Throwable Throwable是 Java 语言中所有错误或异常的超类。 Throwable包含两个子类: Error 和 Exception 。它们通常用于指示发生了异常情况。 Throwable包含了其线程创建时线程执行堆栈的快照，它提供了printStackTrace()等接口用于获取堆栈跟踪数据等信息。
	2. Exception Exception及其子类是 Throwable 的一种形式，它指出了合理的应用程序想要捕获的条件。
	3. RuntimeException RuntimeException是那些可能在 Java 虚拟机正常运行期间抛出的异常的超类。 编译器不会检查RuntimeException异常。 例如，除数为零时，抛出ArithmeticException异常。RuntimeException是ArithmeticException的超类。当代码发生除数为零的情况时，倘若既"没有通过throws声明抛出ArithmeticException异常"，也"没有通过try...catch...处理该异常"，也能通过编译。这就是我们所说的"编译器不会检查RuntimeException异常"！ 如果代码会产生RuntimeException异常，则需要通过修改代码进行避免。 例如，若会发生除数为零的情况，则需要通过代码避免该情况的发生
	4. Error 和Exception一样， Error也是Throwable的子类。 它用于指示合理的应用程序不应该试图捕获的严重问题，大多数这样的错误都是异常条件。 和RuntimeException一样， 编译器也不会检查Error。

	Java将可抛出(Throwable)的结构分为三种类型： 被检查的异常(Checked Exception)，运行时异常(RuntimeException)和错误(Error)。

	- (01) 运行时异常 定义 : RuntimeException及其子类都被称为运行时异常。 特点 : Java编译器不会检查它。 也就是说，当程序中可能出现这类异常时，倘若既"没有通过throws声明抛出它"，也"没有用try-catch语句捕获它"，还是会编译通过。例如，除数为零时产生的ArithmeticException异常，数组越界时产生的IndexOutOfBoundsException异常，fail-fail机制产生的ConcurrentModificationException异常等，都属于运行时异常。 虽然Java编译器不会检查运行时异常，但是我们也可以通过throws进行声明抛出，也可以通过try-catch对它进行捕获处理。 如果产生运行时异常，则需要通过修改代码来进行避免。 例如，若会发生除数为零的情况，则需要通过代码避免该情况的发生！

	- (02) 被检查的异常 定义 : Exception类本身，以及Exception的子类中除了"运行时异常"之外的其它子类都属于被检查异常。 特点 : Java编译器会检查它。 此类异常，要么通过throws进行声明抛出，要么通过try-catch进行捕获处理，否则不能通过编译。例如，CloneNotSupportedException就属于被检查异常。当通过clone()接口去克隆一个对象，而该对象对应的类没有实现Cloneable接口，就会抛出CloneNotSupportedException异常。 被检查异常通常都是可以恢复的。

	- (03) 错误 定义 : Error类及其子类。 特点 : 和运行时异常一样，编译器也不会对错误进行检查。 当资源不足、约束失败、或是其它程序无法继续运行的条件发生时，就产生错误。程序本身无法修复这些错误的。例如，VirtualMachineError就属于错误。 按照Java惯例，我们是不应该是实现任何新的Error子类的！

	对于上面的3种结构，我们在抛出异常或错误时，到底该哪一种？《Effective Java》中给出的建议是： 对于可以恢复的条件使用被检查异常，对于程序错误使用运行时异常。