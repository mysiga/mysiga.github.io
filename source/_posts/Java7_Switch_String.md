title: Java7 switch String
date: 2017-01-25 14:16:29
tags:
categories: Java
---


- Switch string
	- Java 7新特性，原来switch只能支持int，byte。short，char,枚举
- 那具体性能如何了？看下code

````
package com.wilson.test;
public class SwitchTest {
    public static final String NUM_ONE = "1";
    public static final String NUM_TWO = "2";
    public static final String NUM_THREE = "3";
    public static final String NUM_FOUR = "4";
    public static final String NUM_FIVE = "5";
    public static final String NUM_SIX = "6";
    public static final String NUM_SEVEN = "7";
    public static final String NUM_EIGHT = "8";

    public static void main(String[] args) {
        System.out.println("1->switchTime:"+switchOutput("1")+",ifElse:"+ifElseOutput("1"));
        System.out.println("2->switchTime:"+switchOutput("2")+",ifElse:"+ifElseOutput("2"));
        System.out.println("3->switchTime:"+switchOutput("3")+",ifElse:"+ifElseOutput("3"));
        System.out.println("4->switchTime:"+switchOutput("4")+",ifElse:"+ifElseOutput("4"));
        System.out.println("5->switchTime:"+switchOutput("5")+",ifElse:"+ifElseOutput("5"));
        System.out.println("6->switchTime:"+switchOutput("6")+",ifElse:"+ifElseOutput("6"));
        System.out.println("7->switchTime:"+switchOutput("7")+",ifElse:"+ifElseOutput("7"));
        System.out.println("8->switchTime:"+switchOutput("8")+",ifElse:"+ifElseOutput("8"));
    }

    public static long switchOutput(String num) {
        long start = System.currentTimeMillis();
        if (num == null || num.equals("")) {
            return 0;
        }
        for (int i = 0; i < 1000000000; i++) {
            switch (num) {
                case NUM_ONE:
                    break;
                case NUM_TWO:
                    break;
                case NUM_THREE:
                    break;
                case NUM_FOUR:
                    break;
                case NUM_FIVE:
                    break;
                case NUM_SIX:
                    break;
                case NUM_SEVEN:
                    break;
                case NUM_EIGHT:
                    break;
            }
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    public static long ifElseOutput(String num) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < 1000000000; i++) {
            if (num.equals(NUM_ONE)) {
                break;
            } else if (num.equals(NUM_TWO)) {
                break;
            } else if (num.equals(NUM_THREE)) {
                break;
            } else if (num.equals(NUM_FOUR)) {
                break;
            } else if (num.equals(NUM_FIVE)) {
                break;
            } else if (num.equals(NUM_SIX)) {
                break;
            } else if (num.equals(NUM_SEVEN)) {
                break;
            } else if (num.equals(NUM_EIGHT)) {
                break;
            }
        }

        long end = System.currentTimeMillis();
        return end - start;
    }
}

````
开始想当然switch速度更快，那实际输出了?

````
1->switchTime:1868,ifElse:0
2->switchTime:2381,ifElse:0
3->switchTime:985,ifElse:0
4->switchTime:1002,ifElse:0
5->switchTime:486,ifElse:0
6->switchTime:662,ifElse:0
7->switchTime:652,ifElse:0
8->switchTime:661,ifElse:0
````
结果大出我所希望的。用"if-if-else"几乎完胜switch String，那为什么了？
[可以看下这篇文章](http://it.deepinmind.com/java/2014/05/08/how-string-in-switch-works-in-java-7.html),主要switch string，是通过equal和hashcode实现的。String先通过hashcode转换为int，在通过switch int，然后子case里面在if equal String。
- 看来还是回来老路，只是添加一个新的支持特性而效率没有提高。对应String还是用if-if-else吧