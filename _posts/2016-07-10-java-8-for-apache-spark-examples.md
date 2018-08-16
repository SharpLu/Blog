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

##### Intersection

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

##### Distinct

Used to remove the duplicated elements in the RDD, distinct uses the hashCode and equals methods of the objects to find the distincts.



```java
JavaRDD<Integer> rddwithdupElements = javaSparkContext.parallelize(Arrays.asList(1, 1, 2, 4, 5, 6, 8, 8, 9, 10, 11, 11));
JavaRDD<Integer> distinct = rddwithdupElements.distinct();
```

##### Cartesian
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

##### groupByKey

groupByKey() function only works with PairRDD, if your previous RDD is not pair then it doesnt work. It used to group all the values that are related to the keys. It helps the transform a pairRDD consists of <key,value> pairs to pairRDD of <key, Iterable<value>> pairs. The below example, it execute the groupByKey operations on pairRDD generated in the mapToPair() function. 

![](http://feng.io/static/spark_examples/09.png)

```java

JavaPairRDD<String, Integer> pairRDD = intRDD.
mapToPair(i -> (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i));
pairRDD.groupByKey();

After the groupByKey the RDD belongs one key with list of values.

```


##### reduceByKey

##### sortByKey

##### Join

##### coGroup



Actions








 



https://www.packtpub.com/big-data-and-business-intelligence/apache-spark-2x-java-developers