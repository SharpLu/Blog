---
layout: post
title: Configuration IntelliJ to Support Java and Apache Spark
categories:
- blog
tags:
- BigData
date: 2018-07-09
---
In order to started with Apache Spark you need setup your development environment first, here we are briefly introduce how to configuration your IntelliJ to support Java on Spark.
Since majority of our development is based on Java 8 here, 

1.Java jdk 8
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2.https://www.jetbrains.com/idea/


After your finish the installation of JDK and IntelliJ, just following the below steps

1.Open the IntelliJ IDE select the Plugins

![](http://feng.io/static/spark_install/1.png)

2.Search "Scala" language, ensure your development environment will installed Scala.

![](http://feng.io/static/spark_install/2.png)

3.Installed the SBT

![](http://feng.io/static/spark_install/3.png)

4.After finish the installation you need re-start your IDE.

![](http://feng.io/static/spark_install/4.png)	

 Once you start your IDE you can create a new project go to Project structure 

 ![](http://feng.io/static/spark_install/5.png)	
 
https://mvnrepository.com/artifact/org.apache.spark