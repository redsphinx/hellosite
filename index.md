---
layout: page
title: Spark: from dummy to expert
tagline: Gabi Ras
description: this is a test 
---
# The basics
<!--Introduce the reader briefly to spark, and, if you like, the way you carry out the assignment: in the terminal room or at home, deviations from the default suggested commands that you needed to get things running conveniently, etc..-->
## A brief history of big data
The problem of dealing with a large quantity of data begun much earlier than is widely known: it took 8 years to tabulate the 1880 US Census, and this was before the population boom of 1930. After that the problem only got worse as more data about more people was generated. It was not only data about people that had to be kept easily accessible; libraries also had to adapt their storage methods to meet the quickly increasing demand of new publications and research. [This is a nice overview] (http://www.winshuttle.com/big-data-timeline/) depicting milestones in big data. With the invention of the modern computer there was also the invention of multitasking and multiprocessing methods. Before the era of cheap computers, the principal use for multitasking was to allow many people to share the same computer. Big data as we know it today has it's origins in the information influx in the 1960s, when most organisations began to design, develop, and implement centralized computing systems that allowed them to automate their inventory systems. But that was nothing compared to what happened when the internet became mainstream. The study, titled [How Much Information?] (http://www2.sims.berkeley.edu/research/projects/how-much-info/), wrapped up in 1999, a year when the world had produced about 1.5 exabytes of information. [Hadoop] (http://hadoop.apache.org/) was created in 2006 out of the necessity for new systems to handle the explosion of data from the web. Hadoop is largely known for the [HDFS] (https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html#Introduction) and also has a processing component called [MapReduce] (https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html#Overview). In 2014 Spark was developed in response to limitations in the MapReduce cluster computing paradigm. 

## So what is Spark?


Spark is an open source cluster computing framework. It provides an interface for programming entire clusters with implicit data parallelism and fault-tolerance. 
Spark provides programmers with an application programming interface centered on a data structure called the resilient distributed dataset (RDD), a read-only multiset of data items distributed over a cluster of machines, that is maintained in a fault-tolerant way.
It was developed in response to limitations in the MapReduce cluster computing paradigm, which forces a particular linear dataflow structure on distributed programs: MapReduce programs read input data from disk, map a function across the data, reduce the results of the map, and store reduction results on disk. Spark's RDDs function as a working set for distributed programs that offers a (deliberately) restricted form of distributed shared memory. 

<!--Briefly explain what you learned about going through the notebook. Copy the most relevant commands (modified where you thought interesting), and add a brief explanation of what the commands do. (View as report can be a handy feature!)-->

<!--Do not forget to include what you learn from inspecting the Spark UI after issuing commands from the notebook! (Hint: comment on lazy evaluation and/or the effect of caching RDDs.)-->
