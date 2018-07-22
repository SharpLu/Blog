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

A).接口是用implements 关键字，继承是用extends

B).接口与继承的区别，继承只能有一个父类，但是interface 可以implements 很多个class

C).接口和抽象都不可被实例化， 还有就是接口当中所有的“方法”都必须是抽象的.


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

A).父类有的，子类也有

B).父类没有的，子类可以增加

C).父类有的，子类可以改变

继承需要注意的4个原则

A).构造方法不能被继承

B).方法和属性可以被继承

C).子类的构造方法隐式地调用父类的不带参数的构造方法

D).当父类没有不带参数的构造方法时，子类需要使用 super 来显式地调用父类的构造方法， super 指的是对父类的引用


3.多态（Polymorphism）

多态的主要用法简单理解是父类可引用子类的对象， 其次多态的属性包括强制类型转换.

```java
public class PolymorphismTest
{
	public static void main(String[] args)
	{
		Shapes obj = new Shapes(); // 父类的引用指向父类的实例
		obj.draw();

		obj = new Circle(); // 父类的引用指向子类的实例
		obj.draw();

		obj = new Square(); // 父类的引用指向子类的实例
		obj.draw(); 
	}
}

class Shapes
{
	public void draw()
	{
		System.out.println("绘制了一个基本形状。");
	}
}

class Circle extends Shapes
{
	public void draw()
	{
		System.out.println("绘制了一个圆形。");
	}
}

class Square extends Shapes
{
	public void draw()
	{
		System.out.println("绘制了一个正方形。");
	}
}

```
从上例中可以看出，父类的引用指向哪个类的实例就调用哪个类中的方法；

同样是使用父类的引用，调用同一个名称的方法，却可以得到不同的调用结果，这就是Java中的多态，即：同一函数，多种形态；

静态多态

静态多态也称为编译时多态，即在编译时决定调用哪个方法。静态多态一般是指方法重载；只要构成了方法重载，就可以认为形成了静态多态的条件；静态多态与是否发生继承没有必然联系。

动态多态

动态多态也称为运行时多态，即在运行时才能确定调用哪个方法。形成动态多态必须具体以下条件：

多态的强制类型转换

a) 向上类型转换（upcast）：比如说将 Cat 类型转换为Animal 类型，即将子类型转换为父类型。对于向上类型转换，不需要显式指定。

b) 向下类型转换（downcast）：比如将Animal 类型转换为Cat 类型。即将父类型转换为子类型。对于向下类型转换，必须要显式指定（必须要使用强制类型转换）。实际指向的是谁，即可用转换为什么类型的引用。

```java
public class PolyTest2 {
    public static void main(String[] args) {
        Animal a1 = new Dog();
        Dog dog1 = (Dog) a1;
        dog1.sing();

        Animal a2 = new Cat();
        Cat cat = (Cat) a2;
        cat.sing();

        //这里不会有编译错误，但是事实上会执行失败.实际指向的是谁，即可用转换为什么类型的引用。
        Dog dog2 = (Dog)a2;
        dog2.sing();
    }
}


class Animal {
    public void sing() {
        System.out.println("animal is singing");
    }
}

class Dog extends Animal {
    public void sing() {
        System.out.println("dog is singing");
    }
}

class Cat extends Animal {
    public void sing() {
        System.out.println("cat is singing");
    }
}


向上类型转换（upcast）

如下代码所示，可以将cat类型转换为animal类型，即可将子类型转换为父类型，而不需要显示指定
public class PolyTest
{
	public static void main(String[] args)
	{
		Cat cat = new Cat();
		Animal animal = (Animal)cat;  //(Animal)一般省略
		animal.sing();   //打印结果为： "cat is singing"
	}
}

class Animal
{
	public void sing()
	{
		System.out.println("animal is singing");
	}
}

class Cat extends Animal
{
	public void sing()
	{
		System.out.println("cat is singing");
	}
}


向下类型转化（downcast）

如下代码所示，可以将animal类型转换为cat类型，即父类型转换为子类型，此时需要进行强制类型转换。程序能够正常运行，需要两个必要条件：
1)	animal类型的引用指向了cat类型的对象；
2)	使用强制类型转换将animal类型转换为cat类型

public class PolyTest
{
	public static void main(String[] args)
	{
		Animal animal = new Cat();
		Cat cat =(Cat) animal;
		cat.sing();   //打印结果为 "cat is singing"
	}
}

class Animal
{
	public void sing()
	{
		System.out.println("animal is singing");
	}
}

class Cat extends Animal
{
	public void sing()
	{
		System.out.println("cat is singing");
	}
}


```

4.封装（Encapsulation）

封装： 类包含了数据与方法，将数据与方法放在一个类中就构成了封装。
将某些东西包装在一起，然后以新的完整形式呈现出来，隐藏属性、方法或实现细节的处理方式称为封装，封装其实就是有选择性地公开或隐藏某些信息，它解决了数据的安全性问题。

```java
public class EncapTest{

   private String name;
   private String idNum;
   private int age;

   public int getAge(){
      return age;
   }

   public String getName(){
      return name;
   }

   public String getIdNum(){
      return idNum;
   }

   public void setAge( int newAge){
      age = newAge;
   }

   public void setName(String newName){
      name = newName;
   }

   public void setIdNum( String newId){
      idNum = newId;
   }
}
以上实例中public方法是外部类访问该类成员变量的入口。 
通常情况下，这些方法被称为getter和setter方法。 
因此，任何要访问类中私有成员变量的类都要通过这些getter和setter方法。 
通过如下的例子说明EncapTest类的变量怎样被访问：

public class RunEncap{
   public static void main(String args[]){
      EncapTest encap = new EncapTest();
      encap.setName("James");
      encap.setAge(20);
      encap.setIdNum("12343ms");
      System.out.print("Name : " + encap.getName()+ " Age : "+ encap.getAge());
    }
}

```

5.构造函数

构造方法是类里的一个特殊的方法，他不能有返回值（包括void. 所谓构造方法，就是这个类在被实例化时（创建对象时）就要执行的方法，方法名为类的名字，一般的目的是为了给类进行一些初始化值。下面给个栗子


```java
public class pen{
  //成员变量
    private double length;
    private double weigth;
  //构造方法
    public pen(double length){
        this.length=length;
}
    public void pen(double length,double weigth){
        this(length);
        this.weigth=weigth;
    //方法
    public void write(){
    System.out.println("我可以写字")
}
}
}
public static void main(String[] args){
pen ipen=new pen(12.2,2.5)
}

```

构造函数与普通函数的区别在于, 一般函数是用于定义对象应该具备的功能。而构造函数定义的是，对象在调用功能之前，在建立时，应该具备的一些内容。也就是对象的初始化内容。
其次构造函数是在对象建立时由jvm调用, 给对象初始化。一般函数是对象建立后，当对象调用该功能时才会执行。 若普通函数可以使用对象多次调用，构造函数就在创建对象时调用。
这里需要注意构造函数的函数名要与类名一样，而普通的函数只要符合标识符的命名规则即可。 最终注意构造函数没有返回值类型。


Java拷贝构造函数

提到构造函数后，又不得不提到拷贝构造函数，拷贝构造函数就是构造函数的参数的类型是该构造函数所在的类，即参数就是该类的一个对象。example below， 把对象做参数然后通过对象去获取参数进行赋值.


```java

//Example：  
//1.Clock类：  
public class Clock {  
 private int hour;  
 private int minute;  
 private int second;  
   
 public Clock(){  
  setTime(0,0,0);  
 }  
   
 public Clock(int h,int m,int s){  
  setTime(h,m,s);  
 }  
   
 /* 拷贝构造函数 */  
 public Clock(Clock clock){   ///对象做参数
  this.hour=clock.hour;  
  this.minute=clock.minute;  
  this.second=clock.second;  
 }  
   
 public int getHour() {  
  return hour;  
 }  
 public int getMinute() {  
  return minute;  
 }  
 public int getSecond() {  
  return second;  
 }  
 public void setTime(int h,int m,int s){  
  if(h>=0 && h<24)  
   this.hour=h;  
  else  
   this.hour=0;  
    
  if(m>=0 && m<60)  
   this.minute=m;  
  else  
   this.minute=0;  
    
  if(s>=0 && s<60)  
   this.second=s;  
  else  
   this.second=0;  
 }  
   
 public void printTime(){  
  if(this.hour<10)  
   System.out.print("0");  
  System.out.print(this.hour+":");  
    
  if(this.minute<10)  
   System.out.print("0");  
  System.out.print(this.minute+":");  
    
  if(this.second<10)  
   System.out.print("0");  
  System.out.println(this.second);  
 }  
}  
//2.main函数：  
public class Program {  
 public static void main(String[] args) {  
  Clock c1=new Clock(6,43,23);  
  Clock c2=new Clock(c1);//调用拷贝构造函数  
  c1.printTime();  
  c2.printTime();  
 }  
}  
//3.运行结果：  
06:43:23  
06:43:23 

```

