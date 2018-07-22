---
layout: post
title: Java编程语言基础总结 
categories:
- blog
tags:
- Programming
date: 2014-01-21
---

今天来聊一聊关于Java基础的总结，由于语言的局限所以本文就不用蹩脚的英文了. 博主虽然用了Java语言多年但是在工作当中经常还会犯一些基本的错误或则出现一些Bug, 自己简单总结了下还是因为自己在之前在学习Java的时候没有把所有东西都彻底理解清楚. 所以今天来特意写一篇Java总结把Java基础知识概念彻底总结下, 主要针对已经熟悉Java编程语言或则有过一定Java开发经验的人参考,所有知识点均来自Effective Java 3 和圣思园长龙老师的视频教程的课件,若总结不到位请博客联系我进行纠正. 

本文主要围绕Java语言,Java是一种面向对象的语言，面向对象主要的四大特性

1.抽象（Abstract）

抽象在Java里面就是一个概念我们举例说明下. 狗是具体对象，而动物则是抽象概念，猫是具体对象，而动物则是抽象概念，狗和猫都属于动物,但是他们有不同的习性和食物偏好. 所以这时候的抽象概念就会方便与程序设计,简化流程.我们可以通过创建对象狗和猫然后来继承动物的属性. 

简而言之抽象是指将一个Class或则方法进行抽象，抽象之后的方法或则class可以进行继承(Inheritance)但是无法进行(instance)，换句话讲就是无法进行new一个对象. 还有就是抽象方法必须定义在一个抽象的类当中,当然抽象的class可以定义非抽象的方法.

以下代码就会出错，因为方法进行抽象了，但是class并没有抽象所以编译器会报错.

这里Triangle 和 Rectangle 都继承了 Shape class，所以可以直接调用computeArea方法
```java
class Dog                                                       
{                
public abstract void run();    
}

举例抽象的实际应用

public class AbstractTest
{
	public static void main(String[] args)
	{
		Shape shape = new Triangle(10, 6);
		int area = shape.computeArea();
		System.out.println("triangle:" + area);

		shape = new Rectangle(10, 10);
		area = shape.computeArea();
		System.out.println("rectangle:" + area);
	}
}

abstract class Shape
{
	public abstract int computeArea();// 计算形状面积
}

class Triangle extends Shape
{
	int width;
	int height;

	public Triangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}

	public int computeArea()
	{
		return (width * height) / 2;
	}
}

class Rectangle extends Shape
{
	int width;
	int height;

	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}

	public int computeArea()
	{
		return this.width * this.height;
	}
}


```

2.接口（Interface）

接口在Java当中是一个非常重要的概念如果你有接触过RPC RMI等传统Java框架你一定会记得接口这个概念，单独的class实现了多个方法，然后通过接口暴露方法和参数给外在程序进行调用. 相比abstract 接口更容易理解但是其重性可用abstract相当,接口可定义abstract但是默认会省掉. 接口可以按照字面意义去理解，通常接口只为方法提供一个接口其他的方法实现会有另外的class去实现.  

```java
interface Interface1
{
	public void method1();
	public void method2("hi");
	public void method3();
}
abstract class class1 implements Interface1
{
	public void method1(){}
	public void method2(String arg1){
		System.out.println
	}
	public void method3(){}
```
请技术关于如下接口的概念

1.接口是用implements 关键字，继承是用extends

2.接口与继承的区别，继承只能有一个父类，但是interface 可以implements 很多个class

3.接口和抽象都不可被实例化， 还有就是接口当中所有的“方法”都必须是抽象的.


2.继承（Inheritance）

继承是Java当中最基础的一个概念了,同理可以按照字面意义去理解,继承是指一个class可以去继承另外一个class的属性和方法. 被继承的class称为父类，其继承class的类称为子类.

继承的作用：

当今软件设计的特征：

软件规模越来越大；

软件设计者越来越多；

软件设计分工越来越细。

引入继承，实现了代码重用；

引入继承，实现了递增式的程序设计。

继承是能自动传播代码和重用代码的有力工具；

继承能够在某些比较一般的类的基础上建造、建立和扩充新类；

能减少代码和数据的重复冗余度，并通过增强一致性来减少模块间的接口和界面，从而增强了程序的可维护性；

能清晰地体现出类与类之间的层次结构关系。

大家都知道子类可以调用父类class里面的方法，但是调用顺序我想大家未必都清楚.

当生成子类对象时，Java默认首先调用父类的不带参数的构造方法，然后执行该构造方法，生成父类的对象。接下来，再去调用子类的构造方法，生成子类的对象。【要想生成子类的对象，首先需要生成父类的对象，没有父类对象就没有子类对象。比如说：没有父亲，就没有孩子】。
```java
public class Child extends Parent {
    public Child() {
        System.out.println("child");
    }

    @SuppressWarnings("unused")
    public static void main(String[] args) {
        Child child = new Child();
    }
}

class Parent {
    public Parent() {
        System.out.println("no args parent");
    }
}
如果父类没有不带参数的构造方法，则程序此时会出错..
public class Child extends Parent {
    public Child() {
        System.out.println("child");
    }

    @SuppressWarnings("unused")
    public static void main(String[] args) {
        Child child = new Child();
    }
}

class Parent {
    public Parent(int i) {
        System.out.println("parent");
    }
} 
```
继承的主要3个原则

1.父类有的，子类也有

2.父类没有的，子类可以增加

3.父类有的，子类可以改变

继承需要注意的4个原则

1.构造方法不能被继承

2.方法和属性可以被继承

3.子类的构造方法隐式地调用父类的不带参数的构造方法

4.当父类没有不带参数的构造方法时，子类需要使用 super 来显式地调用父类的构造方法， super 指的是对父类的引用


3.多态（Polymorphism）



4.封装（Encapsulation）

5.构造函数


6.Java关键字