---
layout: post
title: How to setup Dask cluster step by step
categories:
- blog
tags:
- BigData
date: 2016-04-28	
---

Setup Dask cluster is cumbersome work, so I like to share the hands-on experience here show how to setup a Dask cluster on Amazon EC2 step by step.

before you start setup the clutser , just ensure you have to setup a Amazon account bind with your credit card, then you can start create your instances 

1. Here we used 4 Ubuntu server for our Dask experiment environment server has 8 cores 32GB memory and 200SSD each instance
2.after ubuntu setup, if you are familiar with Ubuntu shell command then you can start to download the continuum.io  acconda environment and install the Dask and Distributed.

First you have to install annaconda environment  and with Dask

Second make sure you have installed dask distributed lib, this this distributed scheduler, used for coordinate between the client and workers.

```
pip install distributed --upgrade
```

```
conda install distributed -c dask
```

2 cluster

Make sure you  have configured the SSH correctly

Worker machines  `sudo` `apt-get ``install` `openssh-server`

Client machine  `ssh``-keygen`

And  copy the publikey to your client

`ssh``-copy-``id` `-i ~/.``ssh``/id_rsa``.pub client1@192.168.1.2`

`ssh``-copy-``id` `-i ~/.``ssh``/id_rsa``.pub client1@192.168.1.3`

 

if you have done the above, then you can pick one machine for the scheduler,multiple worker and one client

at scheduler machine

executor dscheduler at terminal it will shows,

sparkmaster@ubuntu:~$ dscheduler
distributed.scheduler – INFO – Start Scheduler at: 192.168.253.136:8786
distributed.scheduler – INFO – http at: 192.168.253.136:9786
distributed.scheduler – INFO – Start Bokeh UI at: <http://192.168.253.136:8787/status/>
at worker machines you have to add the worker machines to scheduler

executor dworker ‘192.168.253.136:8786’ at terminal it will shows, depends how many worker machines you have, execute some operation.

Finally at your client you can

from distributed import Executor

executor = Executor(‘scheduler ip and :port’)

```
>>> def square(x):
        return x ** 2

>>> def neg(x):
        return -x

>>> A = executor.map(square, range(10))
>>> B = executor.map(neg, A)
>>> total = executor.submit(sum, B)
>>> total.result()
-285
```

if you can get the result, which means your cluster it works, then you start the next starp.

 

Executor have multiple usage

**submit** used for submit a method with arguments

example as executor.submit(add,(10))

**map**  the map method is pretty similar as the submit

**result**  used for collect the results

**as_completed** used for iterate over results

**get** this method will collect the result by default .example below

```
>>> import dask.array as da
>>> x = da.random.random(1000000000, chunks=(1000000,))
>>> x.sum().compute()  # use local threads
499999359.23511785
>>> x.sum().compute(get=executor.get)  # use distributed cluster
499999359.23511785
```

**compute**  this method can used for many different  scenarios arrays,bags,dataframes so on. It will immediately returns **future** objects by submit or map

**restart**

executor.restart this method used to restart the cluster

 

**precautions**

By default the execution result does not bring results back to your local computer, which leaves them on the distributed network. As a result, computations on returned results like the following don’t require any data transfer.

All communication is between scheduler with workers, the workers with workers without any communication.

Garbage collection

The executor can clean up unused results by reference counting.The executor reference counts `Future` objects. When a particular key no longer has any Future objects pointing to it it will be released from distributed memory if no active computations still require it.
