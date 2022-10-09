### Create a topic with replication factor of 3
`bin/kafka-topics.sh --zookeeper zookeeper1:2181,zookeeper2:2181,zookeeper3:2181/kafka --create --topic fourth_topic --replication-factor 3 --partitions 3`

### Generate 10KB of random data
`base64 /dev/urandom | head -c 10000 | egrep -ao "\w" | tr -d '\n' > file10KB.txt`

> Results:
```
ubuntu@kafka2:~/kafka$ base64 /dev/urandom | head -c 10000 | egrep -ao "\w" | tr -d '\n' > file10KB.txt
ubuntu@kafka2:~/kafka$ ll
-rw-rw-r-- 1 ubuntu ubuntu  9595 Oct  9 16:08 file10KB.txt
```


### In a new shell: start a continuous random producer
`bin/kafka-producer-perf-test.sh --topic fourth_topic --num-records 10000 --throughput 10 --payload-file file10KB.txt --producer-props acks=1 bootstrap.servers=kafka1:9092,kafka2:9092,kafka3:9092 --payload-delimiter A`

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-producer-perf-test.sh --topic fourth_topic --num-records 10000 --throughput 10 --payload-file file10KB.txt --producer-props acks=1 bootstrap.servers=kafka1:9092,kafka2:9092,kafka3:9092 --payload-delimiter A
Reading payloads from: /home/ubuntu/kafka/file10KB.txt
Number of messages read: 148
52 records sent, 10.3 records/sec (0.00 MB/sec), 6.6 ms avg latency, 120.0 max latency.
50 records sent, 10.0 records/sec (0.00 MB/sec), 4.3 ms avg latency, 11.0 max latency.
50 records sent, 9.9 records/sec (0.00 MB/sec), 3.7 ms avg latency, 23.0 max latency.
51 records sent, 10.1 records/sec (0.00 MB/sec), 3.2 ms avg latency, 9.0 max latency.
50 records sent, 10.0 records/sec (0.00 MB/sec), 3.2 ms avg latency, 9.0 max latency.
50 records sent, 9.9 records/sec (0.00 MB/sec), 3.6 ms avg latency, 26.0 max latency.
50 records sent, 10.0 records/sec (0.00 MB/sec), 3.5 ms avg latency, 12.0 max latency.
51 records sent, 10.1 records/sec (0.00 MB/sec), 2.8 ms avg latency, 8.0 max latency.
```

### In a new shell: start a consumer
`bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic fourth_topic`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9092,kafka3:9092 --topic fourth_topic
JSMd91ueER
Yq7jt4LZCzDPCmLjJcFUyYhmShZFu8kNqtSL
eJSj74xHQCe9erlppSLKaxMuj
27Y3WQkYMCl
1b4kX1LiM0Orfj
PDC6WrxEZT53Hhqxitl
LYmjQFPtrBJPBjMKIqFEx0IVwpMg92Cj8DajdXwKp4jfnPyYYkCKG8xfnU8nGrCDSljvjb9IiJ0age
9yMHkqviCF7dJjxEi3jVcbz7i2pWR
IkMYtNIXlX1tLjJjSwos3LOKCb8TSxhpVqZOFKslB77Lmsp1lL2ge0fmJRyCFN6Tm5IsRyHFeT6MRrsUU09ihdW8dVrSsF1OuJKqZer7dyQGR406vQwttKOrdcR
r
.
.
.
.
```

## Test Kafka resiliency
1. kill one kafka server - all should be fine
2. kill another kafka server
3. kill the last server

Now start back the servers one by one


# ERRORS:
## PRODUCER:
```
org.apache.kafka.common.errors.TimeoutException: Expiring 137 record(s) for fourth_topic-0: 30024 ms has passed since batch creation plus linger time
[2017-05-25 10:24:23,784] WARN Error while fetching metadata with correlation id 1086 : {fourth_topic=INVALID_REPLICATION_FACTOR} (org.apache.kafka.clients.NetworkClient)
org.apache.kafka.common.errors.UnknownTopicOrPartitionException: This server does not host this topic-partition.
[2017-05-25 10:24:23,850] WARN Received unknown topic or partition error in produce request on partition fourth_topic-0. The topic/partition may not exist or the user may not have Describe access to it (org.apache.kafka.clients.producer.internals.Sender)
[2017-05-25 10:24:23,914] WARN Error while fetching metadata with correlation id 1092 : {fourth_topic=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
org.apache.kafka.common.errors.NotLeaderForPartitionException: This server is not the leader for that topic-partition.
```

## CONSUMER:
```
[2017-05-25 10:24:23,798] WARN Error while fetching metadata with correlation id 3431 : {fourth_topic=INVALID_REPLICATION_FACTOR} (org.apache.kafka.clients.NetworkClient)
[2017-05-25 10:24:24,081] WARN Auto-commit of offsets {fourth_topic-0=OffsetAndMetadata{offset=3948, metadata=''}} failed for group console-consumer-25246: Offset commit failed with a retriable exception. You should retry committing offsets. (org.apache.kafka.clients.consumer.internals.ConsumerCoordinator)
[2017-05-25 10:24:24,231] WARN Received unknown topic or partition error in fetch for partition fourth_topic-0. The topic/partition may not exist or the user may not have Describe access to it (org.apache.kafka.clients.consumer.internals.Fetcher)
```

Next: [Change Broker Settings](11-change-broker-settings.md)
