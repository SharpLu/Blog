---
layout: post
title: Customize Log4j for Apache Spark 
categories:
- blog
tags:
- BigData
date: 2017-06-08
---

Customize Log4j for Apache Spark

Log4j is a loger framework that has developed by Java, Apache Spark uses log4j as the standard library use to print logs and trace what happens inside the Apache Spark. In many cases that we need to customize the Apache Spark log4j for simplify our logs and also reduce our server disk spaces, in this article it has concluded many different solutions for different scenarios.


As apache spark official documents introduced we only need to replace default configuration file, then Spark will default to looking for the config file and load to application
1.Go to folder "spark 2.0/conf/log4j.properties.template" replace to log4j.properties
Note: log4j.rootCategory=INFO if you change the rootCategory level then it will change all components log level. Customize by yourself


```xml

log4j.rootCategory=INFO, console
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.target=System.err
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %p %c{1}: %m%n

```

Advantage: easy and fast to manipulate

Disadvantage: not available to run in a cluster, try to imagine run your Spark job at over 100X machines cluster do you need to copy your log4j files to all machines?

2.If you want change the log4j.properties file and make it avaibale to run at cluster then you must attach the paramaters to your Spark submit script

```Java
--driver-java-options "-Dlog4j.configurationFile=file:PATH_OF_LOG4J" 
--conf "spark.executor.extraJavaOptions=-Dlog4j.configurationFile=file:PATH_OF_LOG4J" 
--files "/conf/log4j.properties"
```

3.In your Spark application that you could directly use the method to change log4j levels.

Still not available for running in the cluster. Because In the Spark application the only driver will execute the methods and executors do not know the log4j stuff. Either you could use foreachpartition() to invoke this method and make each node and executor execute once the log4j method or use initialization log4j configuration file. Example below


```Java
	public static void main(String[] args) throws IOException {
		File file = new File("D:/log4j2.xml");
		BufferedInputStream in = new BufferedInputStream(new FileInputStream(file));
		final ConfigurationSource source = new ConfigurationSource(in);
		Configurator.initialize(null, source);
		
		Logger logger = LogManager.getLogger("mylog");
	}
```


```Java
import org.apache.log4j.Logger;
import org.apache.log4j.Level;
Logger.getLogger("org").setLevel(Level.OFF);
Logger.getLogger("akka").setLevel(Level.OFF);


```
4.Never tried


```Java
import org.apache.spark.sql.SparkSession;
spark = SparkSession.builder.getOrCreate()
spark.sparkContext.setLogLevel('ERROR')
```

5.As our case it running in EMR cluster, it could use EMR to run one command that passes to all nodes. Simply change the log4j.rootCategory

```Linux
$ sudo sed -i s/log4j.rootCategory=INFO/log4j.rootCategory=WARN/ /usr/lib/spark/conf/log4j.properties
```


6.Copy the configuration file to EMR	 cluster(master node) and attach the file to --file:"/tmp/log4j.properties"

```Linux
scp -i ~/.ssh/emr_dev.pem /Users/alex/Desktop/log4j_files/log4j.properties hadoop@ec2-52-50-131-250.eu-west-1.compute.amazonaws.com:/usr/tmp/

bin/spark-submit --class log4jtest  --files "/usr/tmp/log4j.properties"  --master local[*]  /Users/alex/SparkLog4j/target/SparkLog4j-1.0.jar

```

7.Log4j2.0 in Apache Spark
As log4j2 the performance has significantly improved, so only possible solution to integrate log4j2 to spark solution I could found.

Step 1.Copy all log4j2 jar files to Spark classpath

```Java
log4j-1.2-api-2.9.0.jar
log4j-api-2.9.0.jar
log4j-core-2.9.0.jar
```
Step 2.Copy log4j2.xml to spark conf folder and resources folder

Step 3.Run log4j2 in spark submit command below.

```Java
spark-submit --files "/home/hadoop/resources/log4j2.xml" 
--driver-java-options=-Dlog4j.configurationFile=resources/log4j2.xml 
--conf "spark.executor.extraJavaOptions=-Dlog4j.configurationFile=resources/log4j2.xml"
--conf "spark.driver.extraClassPath=:/home/hadoop/log4j2/*" 
--conf "spark.executor.extraClassPath=:/home/hadoop/log4j2/*"

```

References

http://daizuozhuo.github.io/spark-log4j2/

http://blog.csdn.net/autfish/article/details/51203709

https://logging.apache.org/log4j/2.x/

