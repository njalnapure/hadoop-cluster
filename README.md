
Quickly build arbitrary size Hadoop Cluster based on Docker
------

```
1. Project Introduction
2. Hadoop-Cluster image Introduction
3. Steps to build a 3 nodes Hadoop Cluster
4. Steps to build an arbitrary size Hadoop Cluster
```

##1. Project Introduction

The objective of this project is to help Hadoop developer to quickly build an arbitrary size Hadoop cluster on their local host. This is achieved by using [Docker](https://www.docker.com/).

My project is based on [kiwenlau/hadoop-cluster-docker](https://github.com/kiwenlau/hadoop-cluster-docker) project, however, I've added few more installation apart from Apache Hadoop. I have added HBASE, ZOOKEEPER and SPARK for full use. Hence the size is more Following table shows the differences.

```
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
kiwenlau/hadoop-slave      0.1.0               cbeb1a6de9e8        12 months ago       777.4 MB
kiwenlau/hadoop-master     0.1.0               86c27cc8d9e5        12 months ago       777.4 MB
kiwenlau/hadoop-base       0.1.0               f7fc429ed49d        12 months ago       777.4 MB
kiwenlau/serf-dnsmasq      0.1.0               359ae1f3b69d        12 months ago       206.7 MB
```

```
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
njalnapure/hadoop-slave    latest              619613a4f4a2        11 days ago         5.465 GB
njalnapure/hadoop-master   latest              199bee33814e        11 days ago         5.465 GB
njalnapure/hadoop-base     latest              a795174ba9ad        11 days ago         5.465 GB
njalnapure/serf-dnsmasq    latest              6a919616104b        2 weeks ago         189.7 MB
```
##2. Hadoop-Cluster image Introduction

In this project, I developed 4 docker images: **serf-dnsmasq**, **hadoop-base**, **hadoop-master** and **hadoop-slave**.

#####serf-dnsmasq

Based on ubuntu:15.04. [serf](https://www.serfdom.io/) by HashiCorp and [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) are installed for providing DNS service for the Hadoop Cluster.

#####hadoop-base

Based on serf-dnsmasq, openjdk-8-jdk, openssh-server, vim, wget, scala-2.11.8, Hadoop 2.7.2, HBase 1.1.4, Spark 1.6.1, and Zookeeper 3.4.8 are installed.

#####hadoop-master

Based on hadoop-base. Configure the Hadoop master node.

#####hadoop-slave

Based on hadoop-base. Configure the Hadoop slave node.

Following hierarchy shows the image dependancy:
```
--> Ubuntu:15.04
       |
       |
       --> serf-dnsmasq
                |
                |
                --> hadoop-base
                          |
                          |
                          --> hadoop-master
                          |
                          --> hadoop-slave
```
##3. steps to build a 3 nodes Hadoop cluster

#####a. pull image
```
docker pull njalnapure/hadoop-master:latest
docker pull njalnapure/hadoop-slave:latest
docker pull njalnapure/hadoop-base:latest
docker pull njalnapure/serf-dnsmasq:latest
```

Note**: If you cannot do docker pull directly, try with "sudo docker pull ...."
*check downloaded images*

```
docker images
```

*output*

```
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
njalnapure/hadoop-slave    latest              619613a4f4a2        11 days ago         5.465 GB
njalnapure/hadoop-master   latest              199bee33814e        11 days ago         5.465 GB
njalnapure/hadoop-base     latest              a795174ba9ad        11 days ago         5.465 GB
njalnapure/serf-dnsmasq    latest              6a919616104b        2 weeks ago         189.7 MB
```


#####b. clone source code
```
git clone https://github.com/njalnapure/hadoop-cluster
```
#####c. run container
```
 cd hadoop-cluster
./start-container.sh
```

*output*

```
start master container...
start slave1 container...
start slave2 container...
root@master:~#
```
- start 3 containers，1 master and 2 slaves
- you will go to the /root directory of master container after start all containers

*list the files inside /root directory of master container*

```
ls
```

*output*

```
hdfs  run-wordcount.sh    serf_log  start-hadoop.sh  start-ssh-serf.sh
```

#####d. test serf and dnsmasq service

- In fact, you can skip this step and just wait for about 1 minute. Serf and dnsmasq need some time to start service.

*list all nodes of hadoop cluster*

```
serf members
```

*output*

```
master.njalnapure.com  172.17.0.2:7946  alive
slave1.njalnapure.com  172.17.0.3:7946  alive
slave2.njalnapure.com  172.17.0.4:7946  alive
```
- you can wait for a while if any nodes don't show up since serf agent need time to recognize all nodes

*test ssh*

```
ssh slave2.njalnapure.com
```

*output*

```
root@master:~# ssh slave2.njalnapure.com
Warning: Permanently added 'slave2.njalnapure.com,172.17.0.4' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 15.04 (GNU/Linux 4.1.19-boot2docker x86_64)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@slave2:~#
```

*exit slave2 nodes*

```
exit
```

*output*

```
root@slave2:~# exit
logout
Connection to slave2.njalnapure.com closed.
```
- Please wait for a whil if ssh fails, dnsmasq need time to configure domain name resolution service
- You can start hadoop after these tests!

#####e. start hadoop
```
./start-hadoop.sh
```


#####f. run wordcount
```
./run-wordcount.sh
```

*output*

```
input file1.txt:
Hello Hadoop

input file2.txt:
Hello Docker

wordcount output:
Docker    1
Hadoop    1
Hello    2
```
#####g. start hbase and zookeeper
## make the root directory of hbase
```
hadoop fs -mkdir /hbase
```

## start zookeeper
```
cd <ZOOKEEPER_HOME>
zkServer.sh start
```

## start hbase
```
cd <HBASE_HOME>
start-hbase.sh
```

## TEST
$ jps

##4. Steps to build arbitrary size Hadoop cluster

#####a. Preparation

- check the steps a~b of section 3：pull images and clone source code

#####b. rebuild hadoop-master

```
./resize-cluster.sh 5
```

- you can use any interger as the parameter for resize-cluster.sh: 1, 2, 3, 4, 5, 6...


#####c. start container
```
./start-container.sh 5
```
- you'd better use the same parameter as the step b

#####d. run the Hadoop cluster

- check the steps d~f of section 3：test serf and dnsmasq,  start Hadoop and run wordcount
- please test serf and dnsmasq service before start hadoop
