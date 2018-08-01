---
layout: post
title: Mastering Apache Spark
categories:
- blog
tags:
- BigData
date: 2017-01-05
---

Apache Spark achieves high performance for both batch and streaming data, using a state-of-the-art DAG scheduler, a query optimizer, and a physical execution engine. The basic idea of Spark framework that is used memory as storage load all the data as possible as it can to memory, as we know memory that is significant faster that disk IO. For majority companies organizations their used Apache spark combination with Hadoop, and Hadoop play the role of storage as well as Spark as computation engine. 

Spark as such powerful engine it support various domains big data computation, Spark SQL, MLib, Graphx, Streaming so on.

##### Performance

Spark declare which is 100x faster than Hadoop 2.0, as Hadoop it based on disk and Spark based on memory that make sense. As the major bottleneck of MapReduce is disk IO, and its considerably visible during the shuffle and sort of MR, as data is written to disk. The principal that is spark share the memory cross the cluster and keep everything in memory, of course spark also support persist data to disk,we will discuss this in future. 

##### Fault tolerance

Fault tolerance is essential concept in distributed systems, both Hadoop and Spark their have different approaches to handle fault tolerance. In Hadoop the application master keeps track of mappers and reducers while execute MapReduce jobs. And executors without response in period of time, the application master will requesting the resource master to launches separate JVM to run such tasks. While this approach achieves fault tolerance JVM to run such tasks. 

And Spark computation by RDD(Resilient Distributed Datasets),  and RDD handle failure by lineage graph so that spark will remember how each stage is calculated and if the failure happens it will re-computation the previous stages  and thus making it more resilient.

##### DAG

DAG is Directed Acyclic graph. Spark RDD will generate RDD according to the algorithm and stages execute in sequence. 

##### Spark Core 

A whole Spark framework have build around Spark core to make it more general purpose that RDD which in Spark is simple an immutable distributed collection of objects, each split into multiple partitions.  

##### Compatibility

Spark is a general computation engine which compatible with YARN and Mesos cluster schedule and also support various big data frameworks, like Hadoop, Kafka, so on. Spark also support APIs to access data from cloud like AWS, Google cloud, Azure. 



Hands-on Spark

![](http://feng.io/static/spark_rdd/01.png)



Operations on RDD



Transformation

Action

Laze evaluation

Internal of RDD 

All references from 

https://www.packtpub.com/big-data-and-business-intelligence/apache-spark-2x-java-developers