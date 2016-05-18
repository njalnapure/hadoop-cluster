FROM njalnapure/serf-dnsmasq:latest

MAINTAINER njalnapure <njalnapure@hotmail.com>

# install openssh-server, vim and openjdk
RUN apt-get update -y
RUN apt-get install -y openssh-server vim wget
RUN apt-get install -y openjdk-8-jdk

# move all configuration files into container
ADD files/* /usr/local/

# set jave environment variable
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV PATH $PATH:$JAVA_HOME/bin

RUN addgroup hadoop
RUN useradd -d /home/hduser -m -s /bin/bash -G hadoop hduser
RUN useradd -d /home/hbase -m -s /bin/bash -G hadoop hbase
RUN useradd -d /home/zookeeper -m -s /bin/bash -G hadoop zookeeper
RUN useradd -d /home/spark -m -s /bin/bash -G hadoop spark

#configure ssh free key access
RUN mkdir /var/run/sshd && \
ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
mv /usr/local/ssh_config ~/.ssh/config && \
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

#install Scala 2.11.8

RUN wget http://www.scala-lang.org/files/archive/scala-2.11.8.deb
RUN dpkg -i scala-2.11.8.deb
RUN apt-get update
RUN apt-get install scala
RUN rm -f scala-2.11.8.deb

#install hadoop 2.30

RUN wget http://apache.mirror.anlx.net/hadoop/core/hadoop-2.7.2/hadoop-2.7.2.tar.gz
RUN tar -zxvf hadoop-2.7.2.tar.gz -C /usr/local/
RUN rm -f hadoop-2.7.2.tar.gz
RUN wget http://ftp.twaren.net/Unix/Web/apache/hbase/1.1.4/hbase-1.1.4-bin.tar.gz
RUN tar -zxvf hbase-1.1.4-bin.tar.gz -C /usr/local/
RUN rm -f hbase-1.1.4-bin.tar.gz
RUN wget http://ftp.twaren.net/Unix/Web/apache/zookeeper/zookeeper-3.4.8/zookeeper-3.4.8.tar.gz
RUN tar -zxvf zookeeper-3.4.8.tar.gz -C /usr/local/
RUN rm -f zookeeper-3.4.8.tar.gz
RUN wget http://apache.mirrors.ionfish.org/spark/spark-1.6.1/spark-1.6.1-bin-hadoop2.6.tgz
RUN tar -zxvf spark-1.6.1-bin-hadoop2.6.tgz -C /usr/local/
RUN rm -f spark-1.6.1-bin-hadoop2.6.tgz


RUN ln -s /usr/local/hadoop-2.7.2 /usr/local/hadoop && \
ln -s /usr/local/spark-1.6.1-bin-hadoop2.6 /usr/local/spark && \
ln -s /usr/local/hbase-1.1.4 /usr/local/hbase && \
ln -s /usr/local/zookeeper-3.4.8 /usr/local/zookeeper && \
chown -R hduser:hadoop /usr/local/hadoop-2.7.2 && \
chown -R hduser:hadoop /usr/local/hadoop && \
chown -R hbase:hadoop /usr/local/hbase-1.1.4 && \
chown -R hbase:hadoop /usr/local/hbase && \
chown -R zookeeper:hadoop /usr/local/zookeeper-3.4.8 && \
chown -R zookeeper:hadoop /usr/local/zookeeper && \
chown -R spark:hadoop /usr/local/spark-1.6.1-bin-hadoop2.6 && \
chown -R spark:hadoop /usr/local/spark && \
mv /usr/local/bashrc ~/.bashrc && \
mv /usr/local/hadoop-env.sh /usr/local/hadoop/etc/hadoop/hadoop-env.sh && \
mv /usr/local/hbase-env.sh /usr/local/hbase/conf/hbase-env.sh

RUN chown -R hduser:hadoop /usr/local/hadoop-2.7.2
RUN chown -R hduser:hadoop /usr/local/hadoop
RUN chown -R hbase:hadoop /usr/local/hbase-1.1.4
RUN chown -R hbase:hadoop /usr/local/hbase
RUN chown -R zookeeper:hadoop /usr/local/zookeeper-3.4.8
RUN chown -R zookeeper:hadoop /usr/local/zookeeper
RUN chown -R spark:hadoop /usr/local/spark-1.6.1-bin-hadoop2.6
RUN chown -R spark:hadoop /usr/local/spark
