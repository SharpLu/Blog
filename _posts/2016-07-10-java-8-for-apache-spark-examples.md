---
layout: post
title: Java 8 for Apache Spark Examples
categories:
- blog
tags:
- BigData
date: 2016-07-10
---
This article will discuss about the real cases and examples in Apache spark for Java 8, how to use the common transformation and action methods in Java 8, hope it could help beginnings to get started with Apache Spark.



##### Map()

In map function each elements in the RDD will be mapping to the specific method or value.

![](http://feng.io/static/spark_examples/01.png)

````Java
SparkConf conf = new SparkConf().setMaster("local[*]").setAppName("ApacheSparkJavaDeveloper");
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
List<Integer> intList = Arrays.asList(1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
JavaRDD<Integer> intRDD = javaSparkContext.parallelize(intList, 2);
JavaRDD<Integer> output = intRDD.map(x->x+1);
output.saveAsTextFile("/");
````

#####  Filter()

The filter function used to filter out the specific elements or conditions from RDD.

![](http://feng.io/static/spark_examples/02.png)

````
output.filter(x->(x%2==0))
or
intRDD.filter(x->{
return  x%2==0;
});

````

##### FlatMap()

In flatmap() method, an elements of source RDD could be mapped to one or more elements of target RDD. the function is executed on every elements of source RDD that produces one or more outputs. The return type of flatmap() function is java.util.iterator, that returns a sequence of elements.

![](http://feng.io/static/spark_examples/03.png)

````Java
JavaRDD<String> stringRDD = javaSparkContext.parallelize(Arrays.asList("Hello,Spark", "Hello,Java"));
JavaRDD<String> flatMap = stringRDD.flatMap(t -> Arrays.asList(t.split(",")).iterator());
````

##### mapToPair()

The mapToPair() basically is the same concept with map(), the difference is mapToPair it will return pair object of RDD that consistent of key and value pairs. 

![](http://feng.io/static/spark_examples/04.png)

````
JavaPairRDD<String, Integer> pairRDD = 
intRDD.mapToPair(i -> (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i));
````

##### flatMapToPair()

flatMapToPair is similar to flatMap(), however, the target RDD type is pairRDD which is key value pair, In java you need declare your method for JavaPairRDD<String,Integer>.

![](http://feng.io/static/spark_examples/05.png)

````java
JavaRDD<String> stringRDD = javaSparkContext.parallelize(Arrays.asList("Hello,Spark", "Hello,Java"));
JavaPairRDD<String, Integer> flatMapToPair = stringRD
.flatMapToPair(s -> Arrays.asList(s.split(",")).stream()
.map(token -> new Tuple2<String, Integer>(token, token.length())).collect(Collectors.toList())
.iterator());
````



##### union()

As the meaning of the name, which is union two or more RDDs, the target consist of all the source RDDs, the only difference that is RDDs that are union to one RDD. Example below

![](http://feng.io/static/spark_examples/06.png)

```
JavaRDD<Integer> intRDD1 = javaSparkContext.parallelize(Arrays.asList(1, 2, 4));
JavaRDD<Integer> intRDD2 = javaSparkContext.parallelize(Arrays.asList(1, 4, 5));
JavaRDD<Integer> union = intRDD1.union(intRDD2);
```

##### Intersection()

Intersection It used to find the common  intersected elements of both RDDs, and the target RDD consists of the elements that are identical in sources RDDs.

Example below.

![](http://feng.io/static/spark_examples/07.png)

```java
    public static void main(String[] args) {
        SparkConf conf = new SparkConf().setMaster("local[*]").setAppName("ApacheSparkJavaDeveloper");
        JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
        JavaRDD<Integer> intRDD1 = javaSparkContext.parallelize(Arrays.asList(1, 2, 3 7, 8, 4));
        JavaRDD<Integer> intRDD2 = javaSparkContext.parallelize(Arrays.asList(1, 4, 7, 6, 2,5));
        JavaRDD<Integer> intersection = intRDD1.intersection(intRDD2);
//        intersection.foreach(x -> {
//            System.out.println(x);
//        });

//        JavaRDD<Integer> subtract = intRDD1.subtract(intRDD2);
//       subtract.foreach(x -> {
 //           System.out.println(x);
        });
    }
```

##### Distinct()

Used to remove the duplicated elements in the RDD, distinct uses the hashCode and equals methods of the objects to find the distincts.

![](http://feng.io/static/spark_examples/08.png)
```java
JavaRDD<Integer> rddwithdupElements = javaSparkContext.parallelize(Arrays.asList(1, 1, 2, 4, 5, 6, 8, 8, 9, 10, 11, 11));
JavaRDD<Integer> distinct = rddwithdupElements.distinct();
```

##### Cartesian()
Quick to understand the Certesian() function, this method are part of our mathematical set theory.
More information : https://en.wikipedia.org/wiki/Cartesian_product
Certesian() methods generates a certesian product of two RDDs, each element of our first RDD is paired with each elements of te second RDD. Therefore, the time complexity is n²  and if the certesian operation is executed on an RDD of types X and an RDD of type Y it will return an RDD that will consist of <x,y> pairs. The resultant RDD will consist of all the possible pairs of <x,y>.

Example below
![](http://feng.io/static/spark_examples/09.png)

```java
SparkConf conf = new SparkConf().setMaster("local[*]").setAppName("ApacheSparkForJavaDevelopers");
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
JavaRDD<String> RDD1 = javaSparkContext.parallelize(Arrays.asList("A", "B", "C"));
JavaRDD<Integer> RDD2 = javaSparkContext.parallelize(Arrays.asList(1, 4, 5));
JavaPairRDD<String, Integer> result = RDD1.cartesian(RDD2);
result.foreach(x -> System.out.println(x));



JavaPairRDD<String, String> RDD3 = javaSparkContext.parallelizePairs(Arrays.asList(
new Tuple2("James", "United State"), new Tuple2("Alex", "Franch"),
new Tuple2("Feng", "Italy"), new Tuple2("Jimmy", "United Kingdom")));

JavaPairRDD<String, String> RDD4 = javaSparkContext.parallelizePairs(Arrays.asList(
new Tuple2("IBM", "74544"), new Tuple2("Facebook", "3434343"),
new Tuple2("Oracle", "54545"), new Tuple2("CMS", "454553")));

RDD3.cartesian(RDD4).foreach(x->
System.out.println(x));
```

##### groupByKey()

groupByKey() function only works with PairRDD, if your previous RDD is not pair then it doesnt work. It used to group all the values that are related to the keys. It helps the transform a pairRDD consists of <key,value> pairs to pairRDD of <key, Iterable<value>> pairs. The below example, it execute the groupByKey operations on pairRDD generated in the mapToPair() function. 

![](http://feng.io/static/spark_examples/10.png)

```java

JavaPairRDD<String, Integer> pairRDD = intRDD.
mapToPair(i -> (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i));
pairRDD.groupByKey();

After the groupByKey the RDD belongs one key with list of values.

```
As RDD is partitioned across multiple nodes. A key can be present in multiple partitions. In an operation such as groupByKey data will shuffled cross multiple partitions. So key after. shffling will land on which of the executor running is decided by Partitioner. By default, Spark uses Hash Partitioner that uses hashCode of key to decide which executor it should be shuffed to. User can also provide custom partitioners based on the requirement.

##### reduceByKey()

It used for pairRDDs that helps to aggregate data corresponding to a key with the help of an associative reduce function. We are given the same PairRDD that we are used in groupByKey example, reduceByKey operation can be performed as below.
![](http://feng.io/static/spark_examples/11.png)

```java
List<Integer> intList = Arrays.asList(2,5,7,8);
JavaRDD<Integer> intRDD = javaSparkContext.parallelize(intList, 2);
JavaPairRDD<String, Integer> pairRDD = intRDD.mapToPair(i -> (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i));
pairRDD.reduceByKey((v1, v2) -> (v1 + v2)).foreach(x -> System.out.println(x));
pairRDD.reduceByKey((v1, v2) -> (v1)).foreach(x -> System.out.println(x));
```
    
##### sortByKey()
sortByKey belongs to OrderedRDDFunctions. It used to sort RDD by keys which support ascending or decending orders.
![](http://feng.io/static/spark_examples/12.png)
```java
JavaPairRDD<String, Integer> unsortedPairRDD = javaSparkContext.parallelizePairs(Arrays.asList(new Tuple2<String, Integer>("B", 2), new Tuple2<String, Integer>("C", 5),new Tuple2<String, Integer>("D", 7), new Tuple2<String, Integer>("A", 8)));
JavaPairRDD<String, Integer> result  = unsortedPairRDD.sortByKey(false,1);
result.foreach(x-> System.out.println("sorted "+x));
```
##### Join()


It used to join pair RDDs, this is the same concept as Join in SQL which conbined two or more datasets together. For instance, we have two pair RDDs of <x,y> and <x,z> types. When the join happens it will return an RDD of <x,(y,z)>	 type.

The example below
```java
JavaPairRDD<String, String> pairRDD1 = javaSparkContext
.parallelizePairs(Arrays.asList(new Tuple2<String, String>("B", "A"), new Tuple2<String, String>("C", "D"),
new Tuple2<String, String>("D", "E"), new Tuple2<String, String>("A", "B")));
JavaPairRDD<String, Integer> pairRDD2 = javaSparkContext.parallelizePairs(
Arrays.asList(new Tuple2<String, Integer>("B", 2), new Tuple2<String, Integer>("C", 5),
new Tuple2<String, Integer>("D", 7), new Tuple2<String, Integer>("A", 8)));
JavaPairRDD<String, Tuple2<String, Integer>> joinedRDD = pairRDD1.join(pairRDD2);

pairRDD1.join(pairRDD2).foreach(x -> System.out.println("Join " + x));
pairRDD1.leftOuterJoin(pairRDD2).foreach(x-> System.out.println("leftOuterJoin "+x));
pairRDD1.rightOuterJoin(pairRDD2).foreach(x-> System.out.println("rightOuterJoin "+x));
pairRDD1.fullOuterJoin(pairRDD2).foreach(x-> System.out.println("fullOuterJoin "+x));

In some cases of an RDD partitioned across multiple nodes, same keys will be shuffled to one of the executor nodes for the join operation. Therefore, overload is avaiable for all of the join operations and it will let the user provide a partitioner as well

join(JavaPairRDD<K,V> other,Partitioner partitioner)
leftOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);
rightOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);
fullOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);

```

##### coGroup()


References

https://www.packtpub.com/big-data-and-business-intelligence/apache-spark-2x-java-developers
