FROM njalnapure/hadoop-base:latest

MAINTAINER njalnapure <njalnapure@hotmail.com>

# move all confugration /tmp into container
ADD files/* /tmp/

ENV HADOOP_INSTALL /usr/local/hadoop
ENV HBASE_INSTALL /usr/local/hbase

RUN mkdir $HADOOP_INSTALL/logs

RUN cp /tmp/hdfs-site.xml $HADOOP_INSTALL/etc/hadoop/hdfs-site.xml && \
cp /tmp/core-site.xml $HADOOP_INSTALL/etc/hadoop/core-site.xml && \
cp /tmp/mapred-site.xml $HADOOP_INSTALL/etc/hadoop/mapred-site.xml && \
cp /tmp/yarn-site.xml $HADOOP_INSTALL/etc/hadoop/yarn-site.xml && \
cp /tmp/hbase-site.xml $HBASE_INSTALL/conf/hbase-site.xml

RUN cp /tmp/start-ssh-serf.sh ~/start-ssh-serf.sh && \
chmod +x ~/start-ssh-serf.sh

EXPOSE 22 7373 7946 9000 50010 50020 50070 50075 50090 50475 8030 8031 8032 8033 8040 8042 8060 8088 50060

CMD '/root/start-ssh-serf.sh'; 'bash'
