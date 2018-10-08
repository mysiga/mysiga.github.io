## long*long用什么保存数据

- 前言
 - 突然想起一个问到的面试题：

 	```
 	问：int*int你用什么数据保存？
 	答：用long
 	问：那long*long用什么数据保存？
 	答：应该用String吧
 	后面就没有结果
 	```
其实主要考的int或者long超过了他最多长度你该什么保存数据，这主要后台计算大金额或者大数据的时候会用到。可是问道我这个Android移动开发的，懵逼了。那答案是什么了-->BigDecimal
- [BigDecimal	](https://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html)
	- 一般的float型和Double型数据只可以用来做科学计算或者是工程计算，对精度要求不高。但是在商业计算中，要求的数字精度比较高Java提供了BigDecimal来解决精度不高问题。
	- 先看例子就知道为什么用BigDecimal了
	
	```
	public class BigTests {
    public static void main(String[] args) {
        System.out.println("1111111111 * 1111111111=" + 1111111111 * 1111111111);
        BigDecimal bigDecimal = BigDecimal.valueOf(1111111111);
        System.out.println("BigDecimal.valueOf(1111111111) * BigDecimal.valueOf(1111111111)=" + bigDecimal.multiply(bigDecimal).toString());
        System.out.println("11111111.111 * 11111111.111=" + 11111111.111 * 11111111.111);
        BigDecimal longDecimal = BigDecimal.valueOf(11111111.111);
        System.out.println("BigDecimal.valueOf(11111111.111) * BigDecimal.valueOf(11111111.111)=" + longDecimal.multiply(longDecimal).toString());
    }
}
	```
	输出结果
	
	```
	1111111111 * 1111111111=91750577
BigDecimal.valueOf(1111111111) * BigDecimal.valueOf(1111111111)=1234567900987654321
11111111.111 * 11111111.111=1.2345679012098764E14
BigDecimal.valueOf(11111111.111) * BigDecimal.valueOf(11111111.111)=123456790120987.654321
	```
	很明显通过BigDecimal计算的结果才是正确的计算结果。那我们就知道大数据计算需要用BigDecimal处理了，那也就有两个问题了：
	
	- long为什么计算的结果是错误的？
	- BigDecimal是怎么实现计算的？
