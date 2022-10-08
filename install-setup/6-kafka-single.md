# Kafka Single Node Setup

## Add file limits configs - allow to open 100,000 file descriptors
```
echo "* hard nofile 100000
* soft nofile 100000" | sudo tee --append /etc/security/limits.conf
```

## Reboot for the file limit to be taken into account
```
sudo reboot
sudo service zookeeper start
sudo mkdir /data/kafka`
sudo chown -R ubuntu:ubuntu /data/kafka
```

## Edit kafka configuration
```
rm config/server.properties
vim config/server.properties
```

## Launch kafka
Copy `server.properties` file from `kafka/server.properties`

`bin/kafka-server-start.sh config/server.properties`

## Install Kafka boot scripts
Copy `kafka` file from `kafka/kafka`
```
sudo vim /etc/init.d/kafka
sudo chmod +x /etc/init.d/kafka
sudo chown root:root /etc/init.d/kafka
```

## Ignore the warning if any
`sudo update-rc.d kafka defaults`

## Start kafka
`sudo service kafka start`

## Verify it's working
`nc -vz localhost 9092`

## Look at the server logs
`cat /home/ubuntu/kafka/logs/server.log`


## Create a topic
`bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --create --topic first_topic --replication-factor 1 --partitions 3`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --create --topic first_topic --replication-factor 1 --partitions 3
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic "first_topic".
```

## Produce data to the topic
bin/kafka-console-producer.sh --broker-list kafka1:9092 --topic first_topic
Hello
How are you?
(exit)

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-console-producer.sh --broker-list kafka1:9092 --topic first_topic
Hello
How are you?
^C
```


## Read that data
`bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092 --topic first_topic --from-beginning`
> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092 --topic first_topic --from-beginning
Hello
How are you?
```


## List kafka topics
`bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --list`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --list
__consumer_offsets
first_topic
```

Next: [Kafka multi node cluster](7-kafka-cluster.md)
