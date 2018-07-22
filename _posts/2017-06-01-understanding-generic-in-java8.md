---
layout: post
title: Understanding generic in java 8 
categories:
- blog
tags:
- Programming
date: 2017-06-01
---

Understanding generic in java 8

Since Java 9 will be release in the next month, but it still necessary to share those characteristices and stories of Java 8 in Spark 2.0. I have been used the Java 8 for spark few months, since in some cases it was pretty difficult to find out the examples at Google and Stackoverflow. So today i like to share my experiences of used Java 2.0 for Spark in the past years. Before we discuss this, I have to explain why I like to use Java for Spark not Python and Scala. It because my personally have have some experiences with Java programming, as Spark particularly used to handle Big data as majority of big data sources need connect to different platforms such as IoT, Mysql, Oracle, FTP, devices etc. Because Java exist a lot of reliable solutions for connect to different platforms or sources, it more preferable to use Java as primary language for develop Spark or other Big data solutions. 
To use Java 8 on Apache Spark basically it need to understand the Java generics, interfaces, lambda expressions, lexical scoping, streams, intermediate operations, terminal operations. afterwards its Spark for Java APIs, I will make short discuss of those concepts. 

Generics 

To simplify the generics concept its: Generics used to help the user to create the general purpose code that has abstract type in its definition. That abstract type can be replaced with any concrete type in the implementation. Lets make some example to explain the concept. In this case below the list1 and list2 are inialized with Integer and String type


```java
List<Integer> list1 = new ArrayList<Integer>();
List<String> list2 = new ArrayList<String>();
```

Here list1 is the list of the integers and list2 is list of strings in Java the compiler can express as below, In Java 8 you even dont have to new ArrayList<>() initialize the value in the brackets

```java
List<Integer> list1 = new ArrayList<>();
List<String> list2 = new ArrayList<>();
```
Another case below it brings the compile safety, if without declare the type we could put any object or types in the list, but it will automatically convert as object to store in memory.

```java
List list=new ArrayList<>();
list.add(1);
list.add(2);
list.add("hello");
list.add("there");

Object object = list.get(0);
Integer object = (Integer)list.get(0);
```

The cases below, it inialized with Integer, but we has adhere the "hello", It will throw ClassCastException at runtime. The reason the listGeneric it declared Integer to the JVM, the compiler not allowed to add strings. 
```java
List<Integer> listGeneric =new ArrayList<>();
listGeneric.add(1);
listGeneric.add(2);
listGeneric.add("hello"); // won't compile
```
Lets make an generic type 

example generic type T. Classes will consist of the variable of type T and will also have general purpose method that will return the variable of type T:

```java
public class MyGeneric<T>{
T input;
public MyGeneric(T input){
	this.input = input;
}
public T getInput(){
	return input;
}
}
```

As described above, MyGeneric is the class and it consists of the variable input type T, a contractor to construct the object of the MyGeneric class and initalize the variable input and a method that returns the value of T.

```java
 public class MyGenericsDemo {
     public static void main(String[] args) {
       MyGeneric<Integer> m1 =new MyGeneric<Integer>(1);
       System.out.println(m1.getInput());
       //It will print '1'
       MyGeneric<String> m2 =new MyGeneric<String>("hello");
       System.out.println(m2.getInput());
       //It will print 'hello'
} }

```

Here, we have created two object of the MyGeneric class, one with an integer and string types, when we invoke the getInput method on both objects, they will return the value of their respective type.


References


[Apache spark 2.x for java developers](https://www.packtpub.com/big-data-and-business-intelligence/apache-spark-2x-java-developers)

[对Java Generic相关知识的总结](http://semi-sleep.iteye.com/blog/313556)

[Java深度历险（五）——Java泛型](http://www.infoq.com/cn/articles/cf-java-generics)



