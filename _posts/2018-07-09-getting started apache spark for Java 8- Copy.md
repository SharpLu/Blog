---
layout: post
title: Getting Started Apache Spark
categories:
- blog
tags:
- BigData
date: 2018-07-09
---

Let's getting started with Apache Spark,  before we moving further lets first understand the common terminologies associated with Spark, see how the internal part is works in Spark.

##### Spark components

Driver: 

It used to management the end-to-end execution of Spark jobs or program. The basic idea of driver is first it converts the user program in to tasks and after that it schedule the tasks on the executors. And also,  

Executors:

In any spark job, there must be one or more executors, that is processes the execute smaller tasks delegated by the driver. Executors are work node's processes in charge of running individual tasks in a given Spark job. They are launched at the beginning of Spark application and typically run for the entire lifetime of an application. Once they have run the task they sent the result to the driver. They also provide in memory storage for RDDs that are cached by user programs thought block manager.

Master: 

Apache Spark has been designed in master /slave architecture and hence master refers to the cluster node executing the driver program.

Slave:

Spark is distributed cluster model ,slaves refers to the work nodes which executors are being run on it. And in the spark cluster there could be more slaves but only can be one master.  

Job:

This is a typical word in spark used to descript a specific program. Example  word count.

DAG:

In spark any program or Job could be present to a DAG, the DAG presents the logic of the entire spark job, which is execution in sequential order. Re-computation of RDD in case of a failure is possible lineage can be derived from the DAG.

Tasks:

A job can be split in to smaller units to be operated upon in silos which are called tasks. Each task is executed upon by an executor on a partition of data.

Stages:

Stages is associated to DAG, the DAG has splited the spark logic to multiple stages, which each stages represents a set of tasks having the same shuffle dependencies, that is , where data shuffling occurs. In shuffle map stage the task results are input for the next stage where as in result stage the tasks compute the action that started the real computation of spark job example of count(), take(),sample(),collection() so on.

![](http://feng.io/static/spark_internal/01.png)

The above diagram from spark official documents shows how different Spark components interaction 



![](http://feng.io/static/spark_internal/02.png)

The above diagram shows different phases of Spark job execution:

How spark job get executed:

1.For Spark job it mandatory need to initialize a SparkContext, this is the entrance of Spark application, where SparkContext creates an operator graph of different transformation of job, but once an action gets called on such transformation, the graph gets submitted to the DAGScheduler. Depending on the nature of the RDD or resultant being produced with narrow transformation or wide transformation (those that require the shuffle operation), the DAGScheduler produce stages, according to your Spark application.

2.The DAGScheduler split the DAG into many different stages and each stage comprises of the same shuffle dependency with common shuffle boundaries. 

3.Stages are then  submitted to TaskScheduler as TaskSets by the DAGScheduler. The TaskScheduler schedules the taskSets via cluster manager(YARN,Mesos) and monitor its execution. In case of the failure of the any task, it is re-run an finally the results are sent to the DAHScheduler. In case the result output files are lost, then DAGScheduler resubmits such stages to the TaskScheduler to be rerun again.

4.Finally tasks are then scheduled on the designed executors (JVMs running on slave node) meeting the resource and data locality constrains. Each executor can also have more than one task assigned.