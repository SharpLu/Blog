---
layout: post
title: Mastering Apache Spark 2.0 
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



##### RDD(Resilient Distributed Datasets)

A distributed memory abstraction that perform in-memory computation on large cluster in a fault torrent manner. RDD is immutable data collection and structured as Key Value pairs, it leverage cluster memory and is partitioned cross the cluster.

![](http://feng.io/static/spark_rdd/01.png)

Since RDD is used to processing big data, that spark can be partitioned RDD to cross the cluster , so the below illustrate present of how RDD partitioned to cross in a cluster:

![](http://feng.io/static/spark_rdd/02.png)

RDD support two type of operations, one is transformation and other is called action.

##### Transformation

is a function that produces new RDD from the existing RDDs. It takes RDD as input and produces one or more RDD as output. Each time it creates new RDD when we apply any transformation. Thus, the so input RDDs, cannot be changed since RDD are immutable in nature. 

![](http://feng.io/static/spark_rdd/03.png)

This kind of operation we called transformation since it from one RDD to another RDD, and the below functions is general spark transformation functions.

map(func)
flatMap()
filter(func)
mapPartitions(func)
mapPartitionWithIndex()
union(dataset)
intersection(other-dataset)
distinct()
groupByKey()
reduceByKey(func, [numTasks])
sortByKey()
join()
coalesce()
repartition()



##### Action

Transformation is create a new RDD from each other, but when we want to work with the actual dataset, at that point action is performed. When the action is triggered after the result, new RDD is not formed like transformation. Thus, Actions are Spark RDD operations that give non-RDD values. The values of action are stored to drivers or to the external storage system. It brings laziness of RDD into motion.

An action is one of the ways of sending data from *Executer* to the *driver.* Executors are agents that are responsible for executing a task. While the driver is a JVM process that coordinates workers and execution of the task. The below example is a action example it sum all the values and it will return the result immediately, the the below function list is spark action functions.



![](http://feng.io/static/spark_rdd/04.png)

count()
collect()
take(n)
top()
countByValue()
reduce()
fold()
aggregate()
foreach()



Action must be after transformation RDD, because action is based on the current RDD or previous RDD.

One important aspect of transformation also deals with dependency on partition of the parent RDD. If each partition of the generated RDD depends on fixed partition of the parent RDD then its called narrow dependency. 
##### Laze evaluation

Another important feature for spark that is lazy evaluation, Spark create DAG according to your transformation and action programs. DAG is also called the lineage graph, of all the operations you performs on an RDD. Execution of the graphs starts only when an action is performed on RDD. Lets see the example below.

![](http://feng.io/static/spark_rdd/05.png)

First the RDD read data from storage, it could be HDFS, S3 or local path, and persist the data to RDD1, then RDD2 could be filter some specific data from RDD1 and generated to a new RDD3 that only stored the specific data  and finally could do action to performed the result.

Benefits of RDD is linage , it makes the fault tolerant more reliable and robust, Since spark stored the calculated steps of each RDD stages, in case of any RDD or partition of RDD has failed it no necessary to read data from disk again, while can recalculated the RDD from previous RDD to improve the efficiency.  And Spark knows all the steps to be performed to calculate the end  result in advance, it can leverage this information to maximum to utilize the cluster resources in most optimized manner. 


##### Compare MapReduce to RDD

The best benefits of Spark over Hadoop that since RDD is memory based which can iteratively processing data, instead of MapReduce it will caused IO bottlenecks.

Let's see an example how MapReduce processing data,  the below case. 

Consider running a MR job that read data from HDFS and performs some aggregation and writes the output back to HDFS. Now mapper jobs will read data from HDFS and writes the output to the local file system after completion and Reduce pull that data and runs the reduce process on it . After a while, it will write to the output to HDFS. now, lets say you want to perform another aggregation on the output data and you execute another MapReduce on the output data which you need thought the similar IO again, if you are iteratively 1000x times,  and each time processing 100GB data which caused significant IO waste.   

![](http://feng.io/static/spark_rdd/06.png)

In case of Spark will not perform such heavy IO throughput, for Spark data will read from HDFS once and then spark will perform in memory transformation or RDD for every iteration. The output of every step will be stored in the distributed cluster memory.  In case of the intermediate result is more than the distributed memory size?  that spark will split to disk.

![](http://feng.io/static/spark_rdd/07.png)

 References

https://data-flair.training/blogs/category/spark/

https://www.packtpub.com/big-data-and-business-intelligence/apache-spark-2x-java-developers
