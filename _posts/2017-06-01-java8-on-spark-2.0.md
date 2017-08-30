---
layout: post
title: Java 8 on Spark 2.0 
categories:
- blog
tags:
- BigData
date: 2017-06-01
---

Java 8 on Spark 2.0 

Since Java 9 will release in next month, but it still necessary to share those characteristics and stories for Java 8 in Spark 2.0. I have been used the Java 8  for spark few months, since in some case it was pretty difficult to find out the examples in Google or StackOverFlow, so today i like to share my experiences of used Java 2.0 for Spark in the past few months. Before we discuss this, I have to explain why I like to use Java for Spark not Python or Scala. It because my personally like have have some experiences with Java programming, as Spark particularly used to handle Big data, as majority big data sources need connect to different platforms, IoT, Mysql, Oracle, FTP, devices etc. Because Java exist a lot of reliable solutions for connect to different platforms or sources, it more preferable to use Java as primary language for Spark or Big data solution. 
To use Java 8 on Spark basically it need to understand generics, interfaces, lambda expressions, lexical scoping, streams, intermediate operations, terminal operations. afterwards its Spark for Java APIs. 

Generics 
To simplify the generics concept its: Generics used to help the user to create the general purpose code that has abstract type in its definition. That abstract type can be replaced with any concrete type in the implementation. Lets make some example to explain the concept.

List<Integer> list1 = new ArrayList<Integer>();
List<String> list2 = new ArrayList<String>();
In this case the list1 and list2 are initialized with Integer and String type
List list=new ArrayList<>();
list.add(1);list.add(2);list.add("hello");list.add("there");
In this case it definitely works, because list as defined as object, the data as been stored in the list as Object.

Object object = list.get(0);
Integer object = (Integer)list.get(0);

The case two below, it initialized with Integer, but we has adhere the "hero", It will throw ClassCastException at runtime. The reason the listGeneric it declared Integer to the JVM, the compiler not allowed to add strings. 
List<Integer> listGeneric =new ArrayList<>();listGeneric.add(1);listGeneric.add(2);listGeneric.add("hello"); // won't compile

Lets make an generic type 

example generic type T. Classes will consist of the variable of type T and will also have general purpose method that will return the variable of type T:

public class MyGeneric<T>{
T input;
public MyGeneric(T input){
this.input = input;
}
public T getInput(){
return input;
}
}

As described above, MyGeneric is the class and it consists of the variable input type T, a contractor to construct the object of the MyGeneric class and initalize the variable input and a method that returns the value of T.

 public class MyGenericsDemo {     public static void main(String[] args) {       MyGeneric<Integer> m1 =new MyGeneric<Integer>(1);       System.out.println(m1.getInput());       //It will print '1'       MyGeneric<String> m2 =new MyGeneric<String>("hello");       System.out.println(m2.getInput());       //It will print 'hello'} }


Here, we have created two object of the MyGeneric class, one with an integer and string types, when we invoke the getInput method on both objects, they will return the value of their respective type.


References
[1]:
[2]:
[3]:
[4]:
[5]:
