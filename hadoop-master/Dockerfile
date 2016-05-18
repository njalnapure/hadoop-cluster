FROM njalnapure/hadoop-base:latest

MAINTAINER njalnapure <njalnapure@hotmail.com>

# move all confugration files into container
ADD files/* /tmp/

ENV HADOOP_INSTALL /usr/local/hadoop
ENV HBASE_INSTALL /usr/local/hbase
ENV ZOOKEEPER_HOME /usr/local/zookeeper

RUN mkdir -p ~/hdfs/namenode && \
mkdir -p ~/hdfs/datanode && \
mkdir $HADOOP_INSTALL/logs && \
mkdir /home/zookeeper/data

RUN cp /tmp/hdfs-site.xml $HADOOP_INSTALL/etc/hadoop/hdfs-site.xml && \
cp /tmp/core-site.xml $HADOOP_INSTALL/etc/hadoop/core-site.xml && \
cp /tmp/mapred-site.xml $HADOOP_INSTALL/etc/hadoop/mapred-site.xml && \
cp /tmp/yarn-site.xml $HADOOP_INSTALL/etc/hadoop/yarn-site.xml && \
cp /tmp/slaves $HADOOP_INSTALL/etc/hadoop/slaves && \
cp /tmp/start-hadoop.sh ~/start-hadoop.sh && \
cp /tmp/run-wordcount.sh ~/run-wordcount.sh && \
cp /tmp/start-ssh-serf.sh ~/start-ssh-serf.sh && \
cp /tmp/hbase-site.xml $HBASE_INSTALL/conf/hbase-site.xml && \
cp /tmp/zoo.cfg $ZOOKEEPER_HOME/conf/zoo.cfg && \
cp /tmp/myid /home/zookeeper/data/myid && \
cp /tmp/slaves $HBASE_INSTALL/conf/slaves && \
cp /tmp/hbase-site.xml $HADOOP_INSTALL/etc/hadoop/hbase-site.xml

RUN chmod +x ~/start-hadoop.sh && \
chmod +x ~/run-wordcount.sh && \
chmod +x ~/start-ssh-serf.sh && \
chmod 1777 tmp

# format namenode
RUN /usr/local/hadoop/bin/hdfs namenode -format

EXPOSE 22 7373 7946 9000 50010 50020 50070 50075 50090 50475 8030 8031 8032 8033 8040 8042 8060 8088 50060

CMD '/root/start-ssh-serf.sh'; 'bash'
