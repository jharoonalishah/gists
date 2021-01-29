### Download & requirement
java1.8

https://kafka.apache.org/downloads

### Installation (MAC)
tar xvf kafka_xxx.tgz

### Check
```
cd to/kafka
bin/kafka-topics.sh
```

### Run Zookeeper & Kafka
```
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties

# output
...
[2021-01-27 09:42:15,615] INFO Configuring NIO connection handler with 10s sessionless connection timeout, 1 selector thread(s), 8 worker threads, and 64 kB direct buffers. (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2021-01-27 09:42:15,632] INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2021-01-27 09:42:15,652] INFO zookeeper.snapshotSizeFactor = 0.33 (org.apache.zookeeper.server.ZKDatabase)
[2021-01-27 09:42:15,657] INFO Snapshotting: 0x0 to /tmp/zookeeper/version-2/snapshot.0 (org.apache.zookeeper.server.persistence.FileTxnSnapLog)
[2021-01-27 09:42:15,661] INFO Snapshotting: 0x0 to /tmp/zookeeper/version-2/snapshot.0 (org.apache.zookeeper.server.persistence.FileTxnSnapLog)
[2021-01-27 09:42:15,678] INFO Using checkIntervalMs=60000 maxPerMinute=10000 (org.apache.zookeeper.server.ContainerManager)
...
```
### Change configuration data paths
```
# --------------------
# zookeeper.properties
# --------------------
mkdir /home/username/secure/data/zookeeper
vim config/zookeeper.properties

dataDir=/tmp/zookeeper -> dataDir=/home/username/secure/data/zookeeper

# --------------------
# server.properties
# --------------------
mkdir /home/username/secure/data/kafka
vim config/server.properties

log.dirs=/tmp/kafka-logs -> log.dirs=/home/username/secure/data/kafka
```

### CLI 101
1. Run server `Run Zookeeper & Kafka` section

#### Kafka Topics
bin/kafka.sh # output all command

##### Important command
```
# Create first topic with 3 partitin replication 1
# ** impt: if you have one instance of kafka-server run you can assign
# only 1 replica
kafka-topics --boostrap-server localhost:2181 --topic first_topic --create --partitions 3 --replication-factor 1

# list all topics
kafka-topics --zookeeper localhost:2181 --list

# describe topics
kafka-topics --zookeeper localhost:2181 --topic first_topic --describe

# delete topics
kafka-topics --zookeeper localhost:2181 --topic first_topic --delete
```

### Kafka Push and Receive Message ( Run Producer & Comsumer together to get messages )
```
# ( PRODUCER ) Push messages on a topic
kafka-console-producer --broker-list 127.0.0.1:2181 --topic first_topic
> message 1
> message 2
ctrl+c to exit

# acks=all
kafka-console-producer --broker-list 127.0.0.1:2181 --topic first_topic --producer-property acks=all

# ( CONSUMER ) Receive Message
kafka-console-consumer --bootstrp-server 127.0.0.1:9092 --topic first_topic [--from-beginning]
  - [--from-beginning] : to read message from beginning

```
