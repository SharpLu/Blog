---
layout: post
title: Java 8 for Apache Spark Action Examples
categories:
- blog
tags:
- BigData
date: 2016-07-10
---

In the previous section, we practiced the transformation functions in Spark and today we continues the action functions in Apache Spark.

##### isEmpty ()

isEmpty function it used to return boolean value of a RDD and it could be empty RDD.

```java
SparkConf conf = new SparkConf().setMaster("local")
    .setAppName("ActionExamples");
JavaSparkContext sparkContext = new JavaSparkContext(conf);
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3)); 
boolean isRDDEmpty= intRDD.filter(a-> a.equals(5)).isEmpty();
System.out.println("The RDD is empty ::"+isRDDEmpty);
         
```

#####  Collect()

Collect function it will return a collection of RDD, collection it retrieve data from different partitioned RDD into an array at the driver program. And we need  to attention if our RDD data is too big it might out-of-memory, so collection is suitable for small data.

```java
List<Integer> collectedList= intRDD.collect();
for(Integer elements: collectedList){
   System.out.println( "The collected elements are ::"+elements);
}
```

As the RDD will return an array so we could iteration the array in Java.

##### CollectAsMap()

It will return as key/value map type  dataset, it heritance HashMap characteristics only only key allowed the duplicated Key will be overwrite by other. 

```java
List<Tuple2<String,Integer>> list = new ArrayList<Tuple2<String,Integer>>();
list.add(new Tuple2<String,Integer>("a", 1));
list.add(new Tuple2<String,Integer>("b", 2));
list.add(new Tuple2<String,Integer>("c", 3));
list.add(new Tuple2<String,Integer>("a", 4));
   JavaPairRDD<String,Integer> pairRDD = sparkContext.parallelizePairs(list);
   Map<String, Integer> collectMap=pairRDD.collectAsMap();
for( Entry<String, Integer> entrySet :collectMap.entrySet() ){
   System.out.println("The key of Map is : "+entrySet.getKey()+" and the value is : "+entrySet.getValue());          
}
```

##### Count()

Count it will return an integer number , basically it used to count how many elements that exist in an RDD across partitions.

```java
long countVal=intRDD.count();
System.out.println("The number of elements in the the RDD are :"+countVal);
```

##### CountByKey()

Its same idea as count, but it available for pairRDDs, and the countByKey function it will return a map having RDD element's key as the key of the Map and its value being counted of such key.

```java
public static void main(String[] args) {
    SparkConf conf = new SparkConf().setMaster("local[*]").setAppName("ApacheSparkJavaDeveloper");
    JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
    List<Tuple2<String, Integer>> list = new ArrayList<Tuple2<String, Integer>>();
    list.add(new Tuple2<String, Integer>("a", 1));
    list.add(new Tuple2<String, Integer>("b", 2));
    list.add(new Tuple2<String, Integer>("c", 3));
    list.add(new Tuple2<String, Integer>("a", 4));
    JavaPairRDD<String, Integer> pairRDD = javaSparkContext.parallelizePairs(list);
    Map<String, Long> countByKeyMap= pairRDD.countByKey();
    for( Map.Entry<String, Long> entrySet :countByKeyMap.entrySet() ){
        System.out.println("The key of Map is : "+entrySet.getKey()+" and the value is : "+entrySet.getValue());
    }
}
```

countByValue()

countByValue are available applied to both RDD and pairRDD, and it return correspond dataset. If its RDD it will return specific value in the RDD, instead of if its pairRDD it will return a Map having RDD elements as key and count of occurrence as value:

```java
public static void main(String[] args) {
    SparkConf conf = new SparkConf().setMaster("local[*]").setAppName("ApacheSparkJavaDeveloper");
    JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
    List<Tuple2<String, Integer>> list = new ArrayList<Tuple2<String, Integer>>();
    list.add(new Tuple2<String, Integer>("a", 1));
    list.add(new Tuple2<String, Integer>("b", 2));
    list.add(new Tuple2<String, Integer>("c", 3));
    list.add(new Tuple2<String, Integer>("a", 4));
    JavaPairRDD<String, Integer> pairRDD = javaSparkContext.parallelizePairs(list);
    Map<Tuple2<String, Integer>, Long> countByValueMap= pairRDD.countByValue();
    for( Map.Entry<Tuple2<String, Integer>, Long> entrySet :countByValueMap.entrySet() ){
        System.out.println("The key of Map is a tuple having value : "+entrySet.getKey()._1()+" and "+entrySet.getKey()._2()+" and the value of count is : "+entrySet.getValue());
    }
}
```
