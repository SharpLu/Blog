---
​---
layout: post
title: How to use glances + influxDB + Grafana to implement a monitor to your cluster
categories:
- blog
tags:
- BigData
date: 2016-11-01	
​---
---

The glances + influxDB + Grafana is a good combination to monitor your cluster, since majority cluster monitor is expensive and we could use glances + influxDB + Grafana for deploy a cluster monitor that used to collect your servers status, such as CPU disk network performance throughput so on, give you more accurate of matrices.

The environments based on amazon EC2 Ubuntu desktop

Glances is a monitor plugin developed by python used to monitor your Linux system performances and hardware heaths

please make sure you have installed Python.27 environment before the installation, here I used anaconda environment

```
pip install glances
pip install –upgrade glances
```

![](https://feng.io//static/glances/01.png)

Find out the glances.conf file to configuration connect to influxdb

starting glances make sure it works.

InfluxDB is a opensource database developed by Go . The majority used to store time-series data, such as CPU status per seconds. so on

wget <https://dl.influxdata.com/influxdb/releases/influxdb_1.0.2_amd64.deb>

sudo dpkg -i influxdb_1.0.2_amd64.deb

sudo /etc/init.d/influxdb start

sudo service influxdb start

access your influxdDB by browser localhost:8083

![](https://feng.io//static/glances/02.png)

the glances access your database by port 8086

CREATE DATABASE “glances”

CREATE USER “root” WITH PASSWORD ‘root’ WITH ALL PRIVILEGES

after you configuration all stuff properly then use

glances –export-influxdb  to import data store to influxdb

Try to put your glances.conf file in the ~/.config/glances folder (create it if it did not exist).

Grafana

Grafana is an open source, feature rich metrics dashboard and graph editor for Graphite, Elasticsearch, OpenTSDB, Prometheus and InfluxDB.

Installation steps below

```
wget https://grafanarel.s3.amazonaws.com/builds/grafana_3.1.1-1470047149_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_3.1.1-1470047149_amd64.deb
# … configure your options in /etc/grafana/grafana.ini then
sudo systemctl start grafana-server
$ wget https://grafanarel.s3.amazonaws.com/builds/grafana_3.1.1-1470047149_amd64.deb
$ sudo apt-get install -y adduser libfontconfig
$ sudo dpkg -i grafana_3.1.1-1470047149_amd64.deb
$ sudo service grafana-server start
access your web UI
glances –export-influxdb
sudo service influxdb start
sudo service grafana-server start
```

![](https://feng.io//static/glances/03.png)