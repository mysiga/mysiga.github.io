title: Android使用枚举正确姿态
date: 2017-06-19 18:30:29
tags:
categories: Android

---
- [大家应该都知道Android建议不要用Java枚举，它占用内存很大](https://developer.android.com/topic/performance/memory.html)
 ![图片](http://static.codeceo.com/images/2015/08/0F3D1FC7-6CFC-41D7-9A43-C9A9C9DD45D5.jpg)
 
 那实际开发中肯定是要用Java枚举的，那有没有什么解决办法了。答案肯定是有的，[只是换成另外一种方式](https://developer.android.com/reference/android/support/annotation/StringDef.html)
 
  ```
  /**
 * Android枚举
 */
public class TestEnum {
    /**
     * @Retention(RetentionPolicy.SOURCE) 注解可以告知编译器不将枚举的注解数据存储在 .class 文件中
     *
     */
    /**
     * @StringDef创建整型和字符串集的枚举注解来验证其他类型的代码引用
     * 具体详细看官网{@link https://developer.android.com/studio/write/annotations.html?hl=zh-cn#adding-annotations}
     */
    @Retention(RetentionPolicy.SOURCE)
    @StringDef({
            POWER_SERVICE,
            WINDOW_SERVICE,
            LAYOUT_INFLATER_SERVICE
    })
    //@interface这是Java用来定义一个注解类。
    public @interface ServiceName {}
    public static final String POWER_SERVICE = "power";
    public static final String WINDOW_SERVICE = "window";
    public static final String LAYOUT_INFLATER_SERVICE = "layout_inflater";
    public static String getSystemService(@ServiceName String name) {
        if (POWER_SERVICE.equals(name)) {
            return POWER_SERVICE;
        } else if (WINDOW_SERVICE.equals(name)) {
            return WINDOW_SERVICE;
        } else if (LAYOUT_INFLATER_SERVICE.equals(name)) {
            return LAYOUT_INFLATER_SERVICE;
        }
        return null;
    }
    public static void main(String[] args) {
        //使用
System.out.println(TestEnum.getSystemService(TestEnum.LAYOUT_INFLATER_SERVICE));
    }
}
  ```
  这就是官网的写法了。可是发现没有每次传的参数是
  
  ```
  TestEnum.LAYOUT_INFLATER_SERVICE
  或者
  TestEnum.POWER_SERVICE
  ```
  
  而不是像我们Java枚举的传参
  
  ```
  ServiceName.LAYOUT_INFLATER_SERVICE
  ```
  那是不是需要改造下。[查看资料](https://stackoverflow.com/questions/35625247/android-is-it-ok-to-put-intdef-values-inside-interface)可以这样写
  
```
/**
 * Android枚举
 */
public class TestEnum {
    /**
     * @Retention(RetentionPolicy.SOURCE) 注解可以告知编译器不将枚举的注解数据存储在 .class 文件中
     *
     */
    /**
     * @StringDef创建整型和字符串集的枚举注解来验证其他类型的代码引用 具体详细看官网{@link https://developer.android.com/studio/write/annotations.html?hl=zh-cn#adding-annotations}
     */
    @Retention(RetentionPolicy.SOURCE)
    @StringDef({
            ServiceName.POWER_SERVICE,
            ServiceName.WINDOW_SERVICE,
            ServiceName.LAYOUT_INFLATER_SERVICE
    })
    //@interface这是Java用来定义一个注解类。
    public @interface ServiceName {
        String POWER_SERVICE = "power";
        String WINDOW_SERVICE = "window";
        String LAYOUT_INFLATER_SERVICE = "layout_inflater";
    }

    public static String getSystemService(@ServiceName String name) {
        if (ServiceName.POWER_SERVICE.equals(name)) {
            return ServiceName.POWER_SERVICE;
        } else if (ServiceName.WINDOW_SERVICE.equals(name)) {
            return ServiceName.WINDOW_SERVICE;
        } else if (ServiceName.LAYOUT_INFLATER_SERVICE.equals(name)) {
            return ServiceName.LAYOUT_INFLATER_SERVICE;
        }
        return null;
    }

    public static void main(String[] args) {
        //使用
        System.out.println(TestEnum.getSystemService(ServiceName.LAYOUT_INFLATER_SERVICE));
    }
}
```
不错，就是把常量放到里面去了。这个时候差不多好了，怎么看

```
public static String getSystemService(@ServiceName String name) {
        if (ServiceName.POWER_SERVICE.equals(name)) {
            return ServiceName.POWER_SERVICE;
        } else if (ServiceName.WINDOW_SERVICE.equals(name)) {
            return ServiceName.WINDOW_SERVICE;
        } else if (ServiceName.LAYOUT_INFLATER_SERVICE.equals(name)) {
            return ServiceName.LAYOUT_INFLATER_SERVICE;
        }
        return null;
    }
```
这个代码还是有点怪怪的，在Java中可以通过enum.values()获取所有enum的值，找了好像没有只能通过反射获取所有的public的参数和参数值了

```
public static String getSystemService(@ServiceName String name) {
        Field[] fields = ServiceName.class.getDeclaredFields();
        for (Field field : fields) {
            //在反射对象中设置 accessible 标志允许具有足够特权的复杂应用程序（比如 Java Object Serialization 或其他持久性机制）以某种通常禁止使用的方式来操作对象
            field.setAccessible(true);
            String value = null;
            try {
                value = String.valueOf(field.get(field.getName()));
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
            if (value != null && value.equals(name)) {
                return value;
            }
        }
        return null;
    }
```
ok完成，[代码地址](https://github.com/mysiga/TestProject/blob/master/app/src/main/java/com/mysiga/testproject/TestEnum.java)

这里有几个知识点参考资料链接看下

- [@Retention](@Retention)
- [@StringDef](https://developer.android.com/reference/android/support/annotation/StringDef.html?hl=zh-cn)
- [@interface](http://www.cnblogs.com/bingoidea/archive/2011/03/31/2000726.html)
- [Java反射](http://wiki.jikexueyuan.com/project/java-reflection/java-classes.html)