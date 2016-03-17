---
layout: page
title: Spark, from dummy to expert
tagline: by Gabi Ras
description: it's a trap! 
---
#### A brief history of big data

The problem of dealing with large quantities of data begun much earlier than is widely known: it took 8 years to tabulate the 1880 US Census, and this was before the population boom of 1930. After that the problem only got worse as more data about more people was generated. It was not only data about people that had to be kept easily accessible; libraries also had to adapt their storage methods to meet the quickly increasing demand of new publications and research. [This is a nice overview](http://www.winshuttle.com/big-data-timeline/) depicting milestones in big data. With the invention of the modern computer there was also the invention of multitasking and multiprocessing methods. Before the era of cheap computers, the principal use for multitasking was to allow many people to share the same computer. 

Big data as we know it today has it's origins in the information influx in the 1960s, when most organisations began to design, develop, and implement centralized computing systems that allowed them to automate their inventory systems. But that was nothing compared to what happened when the internet became mainstream. The study, titled [How Much Information?](http://www2.sims.berkeley.edu/research/projects/how-much-info/), wrapped up in 1999, a year when the world had produced about 1.5 exabytes of information. [Hadoop](http://hadoop.apache.org/) was created in 2006 out of the necessity for new systems to handle the explosion of data from the web. Hadoop is largely known for [HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html#Introduction) and also has a processing component called [MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html#Overview) (MR). In 2014 Spark was developed in response to limitations in the MR cluster computing paradigm. 

#### So what is Spark?
<!--Introduce the reader briefly to spark, and, if you like, the way you carry out the assignment: in the terminal room or at home, deviations from the default suggested commands that you needed to get things running conveniently, etc..-->
As mentioned before, Spark was developed in response to limitations in the MR cluster computing paradigm. In a nutshell, it does this by working with a different data structure (RDD) which makes it possible to achieve much higher speeds compared to when working with MR.

Spark is an open source cluster computing framework that provides and API for programming clusters, with implicit data parallelism and fault-tolerance. This API is centered on a data structure called the **resilient distributed dataset** or RDD, a read-only multiset of data items distributed over a cluster of machines that is maintained in a fault-tolerant way.

#### Step 1: Getting started with Spark
When I was learning Spark I followed [this Spark notebook](http://rubigdata.github.io/course/assignments/A2a-spark-101.html) to learn the basics. The first thing to note is that we worked in Scala. I've never worked with Scala before, I am much more comfortable with Python, but I chose to do the Scala notebooks because it is [simply more complete](http://rubigdata.github.io/course/background/spark-notebook.html) than the Python notebook. But there are [better reasons](https://www.linkedin.com/pulse/why-i-choose-scala-apache-spark-project-lan-jiang) to choosing Scala over Python, for example, Scala is in general faster than Python because Spark is built on Scala. It is also possible to code in Java. I am running Ubuntu 14.04 and it was really easy to set up everything I needed to work with the notebook. 

Since you want to learn the basics of Spark, sometimes I will go over key concepts in more detail, sometimes saying the same thing in different words, just to make sure you understand.

##### Understanding the need for RDDs
I've mentioned before that RDD stands for *resilient distributed dataset* and briefly mentioned how it works. But what is an RDD? The easiest way to intuitively get what an RDD is is to first just consider it as a data structure: an abstract object that holds information which we can manipulate. Spark was developed in response to the limitations of MR. When making complex applications and executing complex queries, there is need for efficient primitives for sharing data that MR lacks. In MR the only way to share data across jobs is stable storage, i.e. writing to disk, and that is slow. So the challenge was to design a distributed memory abstraction that is both fault-tolerant and efficient. And so the RDD was born: a restricted form of distributed shared memory, through  immutable, partitioned collections of records and coarse-grained deterministic transformations. An immutable object (unchangeable object) is an object whose state cannot be modified after it is created. Coarse-grained means you can write you transformations to be applied to the whole dataset, but not individual elements on the dataset. Efficient fault recovery happens because Spark caches [lineage](http://stackoverflow.com/questions/30699530/in-apache-spark-how-does-lineage-get-passed-down-in-rdds). That was RDDs in a nutshell and you can find more information about this [over here](http://www.cs.berkeley.edu/~matei/talks/2012/nsdi_rdds.pdf). 

##### Spark UI
A useful feature of Spark is the Spark UI. This is a monitoring tool, by default on port 4040, that displays useful information about your Spark application, such as jobs, stages, RDD size and memory usage and information about running executors. 

##### Basic things to keep in mind
For this part we will take a look at the notebook. The most basic thing is to know how to make an RDD, we initialize an RDD from a collection:

```
val rdd = sc.parallelize(0 to 999,8)
```

Here we call the [SparkContext](http://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.SparkContext)'s `parallelize` method. This is how to create a parallelized collection holding the numbers 0 to 999, split in 8 partitions. SparkContext is the main entry point for Spark functionality. It represents the connection to a Spark cluster, and can be used to create RDDs, accumulators and broadcast variables on that cluster.

RDDs support two types of operations: transformations and actions. Transformations create new datasets from existing ones and actions return values after doing a computation on a dataset. *All transformations in Spark are lazy*, in that they do not compute their results right away. So if we look at the Spark UI there are no [jobs or stages](https://www.mapr.com/blog/getting-started-spark-web-ui) scheduled. When we execute the next command, which is an action and we look at the UI, we can see that there are 2 jobs and stages scheduled. This is because of the laziness, now it executes the transformation and the action:

```
val sample = rdd.takeSample(false, 4)
```

When we called `parallelize`, that was 1 way to create a RDD, so to use a collection in your driver program. Another way to create an RDD is referencing a dataset in an external storage system. In the notebook we do this when we create the Shakespeare textfile:

```
val lines = sc.textFile("data/100.txt.utf-8")
```

##### What's the difference between...?
`map` vs `flatMap`

`map` transforms an RDD of length N into another RDD of length N, while `flatMap` transforms an RDD of length N into a collection of N collections, then flattens these into a single RDD of results. If you are familiar with Python, `flatMap` is kind of like a `split()` and `join()` being done. You would want to do a `flatMap` instead of a `map` when you want your data separated by instance while belonging to the same collection. 

`take` vs `collect`

Usually used when printing the contents of an RDD, these methods serve a similar yet different purpose. Use `take` when you only want to print a few elements of an RDD and use `collect` when you want to print the entire RDD, as `collect` fetches the entire RDD to a single machine.

##### Questions when counting
In the notebook, counting words is used as the running example. At some point 2 questions are asked:

**Q: Explain why there are multiple result files**

A: When we look at the UI, we can see that the stage `saveAsTextFile` exists out of 2 tasks. When we look back at when we create `val words` we can see that we used the `flatMap` and `map` transformations to create it. So, since we have lazy evaluation, they get executed now and 2 output files are created.

**Q: Why are the counts so different?**

A: 

```
val words = lines.flatMap(line => line.split(" "))
              .filter(_ != "")
              .map(word => (word,1))
val wc = words.reduceByKey(_ + _)
wc.filter(_._1 == "Macbeth").collect
```

gives a count of 30, while

```
val words = lines.flatMap(line => line.split(" "))
              .map(w => w.toLowerCase().replaceAll("(^[^a-z]+|[^a-z]+$)", ""))
              .filter(_ != "")
              .map(w => (w,1))
              .reduceByKey( _ + _ )
words.filter(_._1 == "macbeth").collect
  .map({case (w,c) => "%s occurs %d times".format(w,c)}).map(println)
```

gives a count of 284. The reason for this is


