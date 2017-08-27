---
layout: post
title: "Consistency and Availability Trade-off"
categories:
- blog
tags:
- DistributedSystems
date: 2015-10-31
---


With the rapid growth of modern applications data throughput and transactions
requirements, using resilient distributed database systems is getting more and
more popular. Under the background of global busi- ness, the development of
distributed systems should be is geographically scattered and user-friendly,
which means the great potential is hiding in terms of users’ choices. Design of
Distributed systems is complicated, any helpful and evidence- based theories are
necessary whenever implementing, analyzing or using them. Undoubtedly, CAP
theorem[1] is world widely known in this field. Many distributed database
systems have shown their tendency based on application requirements, e.g., RMDBS
are CA based, Cassandra and SimpleDB are AP based, MongoDB and Hbase are CP
based. In this project we will investigate how consistency and availability
conflict with each other in a real distributed system. If the consistency of the
system needs to be strong, then availability will decrease. But if we limit
availability, then consistency will not be guaranteed immediately, but instead
eventually consistency will be reached. We utilize the previously measured the
Geo-Distributed Latencies[2] for the geographically distributed Amazon EC2[3]
server. UDP will be the transport protocol due to its high performance in terms
of low de- lay. Cassandra will be used as the application running in the data
center. A master-slave architecture is used to guarantee consistency in the sys-
tem. The consistency and availability of the data from servers will be
categorized by quorum to a readable format. Our key idea in this project is
going to demonstrate a precise data of the trade off between consistency and
availability and display their relationship on several graphs. Finally, The
graphs will identify different solutions for distributed system users, as they
will have the knowledge of the amount of time wasting or how much they can
sacrifice on time waiting to get stronger consistency.

Introduction

Cloud Computing is as a service that provides dynamic scalable virtualized re-
sources via internet computing model[4], such as access servers, storage,
databases, and other application services. Geographically distributed servers
provide low latency response and ensure the stable security and good performance
for the users around the world. However, these two benefits are not so
compatible as we think. In order to explain the problem above, the CAP theorem
has to be en- gaged inevitably, Consistency (each server returns the correct
response to each request), Availability (getting a response to each request),
and Partition tol- erance (fault tolerance when system partitioned into multiple
groups). So, in- compatible problem becomes the conflict between consistency and
availability. High availability is the basic requirement for any online service,
of which num- bers of replicas are the basic premise. Though replicating date
consumes time resources. However, on the positive side, widely used
non-relational database gives us greater flexibility in coordinating consistency
and availability, hence customized distributed systems are becoming typical,
users’ patterns of utilization have changed from passive receiving to positive
selection. Partition tolerance is an essential requirement for any distributed
system. In this paper, we use Amazon EC2 as the cloud computing service. To make
data realistic, several servers are geographically distributed to some of the
available regions in the real world and measure round trip time between each of
these data centers. Finally, using these set of textual data(RTT) obtained by
multiple scenarios to make the trade-off visible and selectable.

2 Theoretical framework

In 1999, the CAP principle was published and presented as a conjecture by
Brewer, and three years later it was proved by Seth Gilbert and Nancy Lynch.
Although the development of distributed systems started decades ago, the CAP
theorem was not taken seriously at the beginning. However, the massive data
processing and storage stimulates the application of distributed database sys-
tems, which made theorem regain focus, and generated a huge controversy. Figure
1 shows the popular databases categorized by CAP principle.

![Figure 1: Popular databases categorized by CAP theory.](http://feng.io/static/distributed/CAPtheory.png)

The CAP theorem states that it is impossible to simultaneously provide
guarantees of consistency, availability, and partition tolerance. Due to the
Geo- distributed system in our project, partition tolerance is the default
attribute that should be guaranteed. Since the partition is involved, the
unstable network latency in the wide area network (WAN) should be taken into
account. Another factor that causes the decrease of availability is replication.
For now, there are only three alternatives for implementing data replication[5]:
1.Update data sent to all replicas at the same time. 2. Data updates sent to an
agreed-upon location first. 3. Data updates sent to an arbitrary location first.
All of these strategies can not avoid increasing time consumption. For
simulating the real world test, consistency is achieving the desired quo- rum,
while availability is a factor of the maximum acceptable latency. In order to
avoiding consistency waste and high latency causing unpredictable waiting, it is
necessary to detect how much consistency can be traded for availability and vice
versa.

3 Hypothesis

The trade-off between consistency and availability can be demonstrated by
graphs, which will show the precise relationship between consistency require-
ments and time wasted in the distributed system. Based on this users will
positively generate or control their own strategy on time consumption and re-
source guarantee.

4 Method

Amazon EC2 is used for providing the cloud service, which can offer scalable
computing capacity in the Amazon Web Services (AWS) cloud [3]. Amazon EC2
resources are world-wide distributed, they are located in 9 regions, namely Asia
Pacific (Tokyo), Asia Pacific (Singapore), Asia Pacific (Sydney), EU (Frank-
furt), EU (Ireland), South America (Sao Paulo), US East (N. Virginia), US West
(N. California), US West (Oregon) 9 regions. Each region is completely isolated
from other regions, which yield the greatest possible fault tolerance and
stability. Cassandra[6] distributed database system was deployed on Amazon EC2
as data center in each different region. Cassandra is an open source distributed
database management system designed to handle large amounts of data across many
commodity servers, providing high availability with no single point of failure.
The feature of high configurable on replication strategy gives more flexibility
on consistency choosing level,which means there are many replica selection
algorithms are provided, each Snitch[7] module offers the most efficient way of
replica locating for different purposes. In this case, Dynamic Snitch[8] is
selected automatically chooses the closest nodes. In consistency level, “Data
updates sent to an agreed-upon location first”[5] data replication strategy is
used, which can be understood as master-slaves structure. As Figure 2 shows,
master node must wait to be synchronized all slaves node, then next round will
be continued. Each data center performs a single node in a connected topology, A
master node can select K nearest replicas(nodes) to communicate with, such as
sending requests or updating data. We calculate the time it takes until a sent
request is received the reply from target. The time is consist of round trip
time in network transmission plus processing time in the remote data centers.
Comparing the performances of time consumption in network and passing routers
between three network transport protocol(ICMP, TCP and UDP), we conclude to use
UDP as primary choice due to its high performance in terms low delay.

![Figure 2: Master-slaves structure for guarantee consistency in distributed sys- tems.](http://feng.io/static/distributed/master-slaves.png)

A great deal of experiment data will be collect from a server. We will then
categorize these data by quorum into a readable format. Based on obtaining the
C(quorum number) and A(latency) data, we will analyze cumulative distri- bution
function(CDF) graphs generated by using Matlab.

5 Results and Analysis There were 8 files of latency data per region collected,
each corresponds to a particular consistency level. Each file contains 600 000
latency samples. Data has been recorded from Mon, 25 May 2015 22:02:01 GMT to
Thu, 04 Jun 2015 05:50:24 GMT. Data demonstrates optimal (minimal) latency
achievable when running ser- vice with a particular consistency from a
particular data center. We mainly analyze the data centers which are located in
Ireland, Tokyo and California. Evenly separate the world into three pieces, the
potential benefit on location selection is easy to understand. Figure 3 shows
Round Trip Time (RTT) in three geographical data centers when different quorum
participates. In these graphs we focus on the percentage of consistency above 99
percent, because after monitoring, we find that the system reaches consistency
in an extremely short time under this point and there is no significant
fluctuation also.

6 Discussion After analyzing all the graphs, we can see that in this distributed
system the slope is changing bigger when the consistency requirement is higher
than 99 percent or more, the trade off can be more obvious. Based on this
perspective, we conclude that in every distributed system there always exists a
critical point when analyzing the consistency and availability. Furthermore, to
the application level, when users demand for consistency reaches over a
threshold, the trade off between consistency and availability will become more
meaningful and worthy. So, according to our graphs, users are provided several
trade off strategies, which avoiding unpredictable or endless time waiting and
consistency waste in a durable time limit, therefor saving the cost.

![Figure 3: The latency RTT in three geographical data centers when different quorum participates.](http://feng.io/static/distributed/latencyRTT.png)

6.1 Latency In Figure 3(C), when we allocate 7 replicas to the master which is
in California, we can realize that 99 percent of nodes reach consistency in less
than 200ms, which means the conflict occurs when higher consistency needs to be
satisfied. Obviously, with the growth of consistency, the increasing of time
wasting is not monotonous. A significant feature of the online service is delay
zero tolerance, so it is necessary for users to know whether the time consuming
is worthy or consistency waste. Unpredictable high latency may cause bad user
experience result in serious users lost or turn to other services.

6.2 Consistency Although over 99 percent of nodes can reach consistency showed
by all graphs, in the real world application that is far from enough. Massive
data throughput amplifies the affect in consistency flaw, sometimes immeasurable
loss triggered, because of 0.001 percent of consistency not reached, eg., stock
system, bank system. So through analyzing our graphs, users can exactly know
within an acceptable time limitation what is the best performance of
consistency.

6.3 Definitions We find that there exists a practical and measurable data
relationship between desired consistency and acceptable time waiting for
participants to coordinate. The trade off between consistency and availability
in geographically distributed system can be shown by graphs with precise data.
6.4 Questions Unstable network environment can not be ignored in partition
network. Some wired inflection points may be caused by network congestion, and
different server load also affect the processing speed. 7 Conclusion In this
paper, we first deployed Cassandra distributed database system on the
geographically distributed cloud service, Amazon EC2. Then we use Snitch to
achieve master node communication with the nearest K slave nodes. Finally, we
use sets of latency data, which categorized by quorum number to generate CDF
graphs by using Matlab. Through analyzing graphs, we find that the trade off
between consistency and availability is configurable and visible to users,but
not always needed.

References

[1] Seth Gilbert,Nancy Lynch. Brewer’s conjecture and the feasibility of consis-
tent, available, partition-tolerant web services. ACM SIGACT News, 33:51– 59,
June 2002.

[2] Kirill Bogdanov,Miguel Peo ́on-Quiro ́os,Gerald Q. Maguire Jr.Dejan Kostic ́o.
The nearest replica can be farther than you think. SoCC ’15 Proceedings of the
Sixth ACM Symposium on Cloud Computing, pages 16–29, 2015. ISBN 0-13-046109-1.

[3] Amazon ec2. http://aws.amazon.com/ec2/.

[4] Qusay F. Hassan. Demystifying cloud computing. Computer, Febru- ary 2011.
http://static1.1.sqspcdn.com/static/f/702523/10181434/
1294788395300/201101-Hassan.pdf.

[5] Daniel Abadi. Consistency tradeoffs in modern distributed database system
design: Cap is only part of the story. Computer, 45:37–42, February 2012.

[6] Apache. cassandra. http://cassandra.apache.org/. [7] Apache. cassandra,
snitch types. http://docs. datastax.com/en/cassandra/2.0/cassandra/architecture/
architectureSnitchesAbout_c.html. [8] BRANDON WILLIAMS. Dynamic snitching in
cassandra: past, present, and future. AUGUST 2012.
http://www.datastax.com/dev/blog/
dynamic-snitching-in-cassandra-past-present-and-future.
