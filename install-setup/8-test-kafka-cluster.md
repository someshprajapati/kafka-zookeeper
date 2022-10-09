### We can create topics with replication-factor 3 now!
`bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --create --topic second_topic --replication-factor 3 --partitions 3`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --create --topic second_topic --replication-factor 3 --partitions 3
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic "second_topic".
```

### We can publish data to Kafka using the bootstrap server list!
`bin/kafka-console-producer.sh --broker-list kafka1:9092,kafka2:9092,kafka3:9092 --topic second_topic`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-console-producer.sh --broker-list kafka1:9092,kafka2:9092,kafka3:9092 --topic second_topic
Hi
Welcome back
Kafka setup testing
it's working
^C
```


### We can read data using any broker too!
`bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic second_topic --from-beginning`

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic second_topic --from-beginning
Hi
it's working
Welcome back
Kafka setup testing
^CProcessed a total of 4 messages
```

### We can create another topics with replication-factor 3 now!
`bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --create --topic third_topic --replication-factor 3 --partitions 3`

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --create --topic third_topic --replication-factor 3 --partitions 3
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic "third_topic".
```

### Let's list topics
`bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --list`

> Results:
```
ubuntu@kafka3:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --list
__consumer_offsets
first_topic
second_topic
third_topic
```

### Publish some data
`bin/kafka-console-producer.sh --broker-list kafka1:9092,kafka2:9092,kafka3:9092 --topic third_topic`

> Results:
```
ubuntu@kafka3:~/kafka$ bin/kafka-console-producer.sh --broker-list kafka1:9092,kafka2:9092,kafka3:9092 --topic third_topic
Hi
publishing the data to third topic
hope it will work
ya, it's working
^C
```


### We can read data using any broker too!
`bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic third_topic --from-beginning`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic third_topic --from-beginning
Hi
ya, it's working
publishing the data to third topic
hope it will work
^C
```


### Let's delete `third_topic` topic!
`bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --delete --topic third_topic`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --delete --topic third_topic
Topic third_topic is marked for deletion.
Note: This will have no impact if delete.topic.enable is not set to true.
```

### Verify if topic `third_topic` deleted successfully!
bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --list

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --list
__consumer_offsets
first_topic
second_topic
```
### Verify the file system, Check `second_topic` created using the replication-factor 3 and partitions 3!
```
ubuntu@kafka1:~/kafka$ ls -l /data/kafka/
total 240
-rw-r--r-- 1 root   root      4 Oct  9 11:32 cleaner-offset-checkpoint
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 __consumer_offsets-0
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 __consumer_offsets-1
.
.
.
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 __consumer_offsets-9
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 first_topic-0
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 first_topic-1
drwxr-xr-x 2 root   root   4096 Oct  9 11:02 first_topic-2
-rw-rw-r-- 1 ubuntu ubuntu   54 Oct  8 17:19 meta.properties
-rw-r--r-- 1 root   root   1294 Oct  9 11:37 recovery-point-offset-checkpoint
-rw-r--r-- 1 root   root   1297 Oct  9 11:37 replication-offset-checkpoint
drwxr-xr-x 2 root   root   4096 Oct  9 11:23 second_topic-0
drwxr-xr-x 2 root   root   4096 Oct  9 11:23 second_topic-1
drwxr-xr-x 2 root   root   4096 Oct  9 11:23 second_topic-2


ubuntu@kafka1:~/kafka$ ls -l /data/kafka/second_topic-0/
total 4
-rw-r--r-- 1 root root 10485760 Oct  9 11:23 00000000000000000000.index
-rw-r--r-- 1 root root       46 Oct  9 11:25 00000000000000000000.log
-rw-r--r-- 1 root root 10485756 Oct  9 11:23 00000000000000000000.timeindex
```

Next: [Kafka Manager](9-tools-kafka-manager.md)
