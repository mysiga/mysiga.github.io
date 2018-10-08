title: Java - Java基础
date: 2018-06-20  11:30:00
tags:
categories: Java
---


![Java](https://upload-images.jianshu.io/upload_images/1534431-82e3766a45c98f7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###["=="和equals区别](http://www.importnew.com/6804.html)
  "=="是比较的内存地址，equals是Object的方法，默认是比较两个对象内存地址，具体看对象是否重写equals方法。
###[equals和hashCode](https://www.cnblogs.com/Qian123/p/5703507.html)
使用Hashset、Hashmap、Hashtable不允许添加重复元素，为了提高效率首先判断元素生成的hashcode()是否相同，如果生成的hashCode()不同在判断equls()方法。
###String、StringBuffer与StringBuilder
||线程安全|序列化|默认长度|增长倍数|
|-|-|-|-|-|
|String|线程安全|支持序列化|0|-|
|StringBuffer|线程安全|支持序列化|16|2n+2|
|StringBuilder|线程不安全|支持序列化|16|2n+2|
###List子类
| List | 实现 | 默认容器大小  | 容量扩充 | 线程是否安全 | 元素是否可以为null |是否支持序列化|优势|
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | 
| Vector | 数组实现 | 10  | 2倍 | 安全 | 可以 |否|已经基本不再使用|
| ArrayList | 数组实现 | 10  | 2倍 | 不安全 | 可以 |是|查找效率高，插入或删除元素效率低|
| LinkedList | 双向循环链表 | 0  | 不需要扩容 | 不安全 | 可以 |是|插入删除效率高，查找效率低|
###Set子类
###Map子类
| Map | 实现 | 默认容器大小  | 容量扩充 | 线程是否安全 | 元素是否可以为null |是否支持序列化|优势|
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | 
| [HashMap](https://github.com/francistao/LearningNotes/blob/master/Part2/JavaSE/HashMap源码剖析.md) | 哈希表实现的 | 16  | 2倍| 不安全 |key和value都允许为null，如果key为null，则直接从哈希表的第一个位置table[0]对应的链表上查找。记住，key为null的键值对永远都放在以table[0]为头结点的链表中，当然不一定是存放在头结点table[0]中, |支持|如果单链表中存在与目标key相等的键值对，则将新的value覆盖旧的value|
| LinkedHashMap | 哈希表实现的 | 16  | 2倍 | 不安全 | 不可以 |支持||
| Hashtable | 哈希表实现的 | 16  | 2倍+1 | 安全 | key可以为null，value不允许为null |支持||

###int-char-long各占多少字节数

| 类型 | 位数 | 字节 |
| :------  | :------ | :------ |
| byte    | 8       | 1       |
| char    | 16     | 2     |
| short   | 16     | 2       |
| int       | 32     | 4       |
| long    | 64     | 8      |
| float    | 32     | 4       |
| double| 64     | 8      |

- [自定义Java注解处理器](https://yuweiguocn.github.io/java-annotation-processor/)
- [Java 23种设计模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
    - 单例模式
    - [工厂模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
       1. [静态工厂模式](http://www.cnblogs.com/lcw/p/3802790.html)
       2. [工厂方法模式](http://wiki.jikexueyuan.com/project/java-design-pattern/factory-pattern.html)
       3. [抽象工厂模式](http://wiki.jikexueyuan.com/project/java-design-pattern/abstract-factory-pattern.html)
       4. 区别：
          > 静态工厂模式:用静态方法获取指定产品
             工厂方法模式:定义工厂接口,一个方法,让不同的子类工厂生产指定产品
             抽象工厂模式:同工厂方法模式类似,只是工厂接口有多个方法

    - 建造者模式
    - 代理模式
    - 策略模式
    - 模板模式
    - 观察者模式

#### 网络
- [HTTP,TCP,Socket关系](http://blog.csdn.net/bjyfb/article/details/6682913)
 - [TCP和UDP区别？TCP几次握手，几次断开？](http://mp.weixin.qq.com/s?__biz=MzA4MzQxMzg5MA==&mid=2649830415&idx=2&sn=760da2138f07c96ef25d451d745c4c72&scene=0#wechat_redirect)
