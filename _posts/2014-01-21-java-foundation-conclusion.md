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

2.继承（Inheritance）

3.多态（Polymorphism）

4.封装（Encapsulation）

