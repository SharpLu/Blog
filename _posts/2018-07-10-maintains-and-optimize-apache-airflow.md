---
layout: post
title: "Maintains and Optimize Apache Airflow"
categories:
- blog
tags:
- BigData
date: 2018-07-10
---

Airflow is a platform used to maintains and schedule your ETL pipelines by DAG. It support various of languages to execute such as Python, bash so on. The basic idea from Airflow is to used a DAG  (directed acyclic graphs) to schedule all the tasks if any nodes inside the DAG has fail the dependences won't execute.

Today we are focused on the optimize and maintains, after our big data team used airflow a while the airflow have too much logs and database that serious impacted our performance and server because we are deployed hundreds of tasks on the airflow. 

Thanks for the open source community there have some DAGS that could implemented to the Airflow and scheduled to maintains your airflow automatically 
Link below
https://github.com/teamclairvoyant/airflow-maintenance-dags

Before you deploy to your production here suggest your make a test in your local machine, make it works first.

Steps to install airflow on Mac

brew install python python3

pip install airflow

mkdir ~/airflow

# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=~/airflow

# install from pypi using pip
pip install airflow

# initialize the database
cd ~/airflow && airflow initdb

# start the web server, default port is 8080
cd ~/airflow && airflow webserver -p 8080

After setup the airflow, then you can deploy following the below steps.

Basic steps

1.Login to the machine running Airflow

2.Navigate to the dags directory

3.Copy the airflow-db-cleanup.py file to this dags directory

wget https://raw.githubusercontent.com/teamclairvoyant/airflow-maintenance-dags/master/db-cleanup/airflow-db-cleanup.py

4.Update the global variables in the DAG with the desired values

```python
DAG_ID = os.path.basename("your file name").replace(".pyc", "").replace(".py", "")  # airflow-db-cleanup
```
5.Enable the DAG in the Airflow Webserver

Test passed on airflow versions.
1.7
1.8
1.9

![](http://feng.io/static/airflow/1.png)

