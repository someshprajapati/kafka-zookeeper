## DO THE SAME FOR BROKER 2 and 3

## Add file limits configs - allow to open 100,000 file descriptors
```
echo "* hard nofile 100000
* soft nofile 100000" | sudo tee --append /etc/security/limits.conf
```

## Reboot for the file limit to be taken into account
```
sudo reboot
sudo service zookeeper start
sudo mkdir /data/kafka
sudo chown -R ubuntu:ubuntu /data/kafka
```

## Edit kafka configuration
```
rm config/server.properties
vim config/server.properties
```

## Launch kafka
Copy `server.properties` file from `kafka/server.properties`.

> NOTE: Please make sure `broker.id` and `advertised.listeners` are updated on each kafka server.

`bin/kafka-server-start.sh config/server.properties`

> Results:
```
ubuntu@kafka1:~/kafka$ grep "broker.id\|listen\|replicas" config/server.properties
broker.id=1
advertised.listeners=PLAINTEXT://kafka1:9092
min.insync.replicas=2

ubuntu@kafka2:~/kafka$ grep "broker.id\|listen\|replicas" config/server.properties
broker.id=2
advertised.listeners=PLAINTEXT://kafka2:9092
min.insync.replicas=2

ubuntu@kafka3:~/kafka$ grep "broker.id\|listen\|replicas" config/server.properties
broker.id=3
advertised.listeners=PLAINTEXT://kafka3:9092
min.insync.replicas=2
```


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

> Results:
```
ubuntu@kafka2:~/kafka$ sudo service kafka start

ubuntu@kafka3:~/kafka$ sudo service kafka start
```

## Verify it's working
`nc -vz localhost 9092`

> Results:
```
ubuntu@kafka3:~/kafka$ nc -vz kafka1 9092
Connection to kafka1 9092 port [tcp/*] succeeded!

ubuntu@kafka3:~/kafka$ nc -vz kafka2 9092
Connection to kafka2 9092 port [tcp/*] succeeded!

ubuntu@kafka3:~/kafka$ nc -vz kafka3 9092
Connection to kafka3 9092 port [tcp/*] succeeded!
```

## Look at the server logs
`cat /home/ubuntu/kafka/logs/server.log`


## Make sure to fix the __consumer_offsets topic
`bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --config min.insync.replicas=1 --topic __consumer_offsets --alter`

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-topics.sh --zookeeper zookeeper1:2181/kafka --config min.insync.replicas=1 --topic __consumer_offsets --alter
WARNING: Altering topic configuration from this script has been deprecated and may be removed in future releases.
         Going forward, please use kafka-configs.sh for this functionality
Updated config for topic "__consumer_offsets".
```


## Read the topic on broker 1 by connecting to broker 2!
`bin/kafka-console-consumer.sh --bootstrap-server kafka2:9092 --topic first_topic --from-beginning`

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka2:9092 --topic first_topic --from-beginning
How are you?
Hello
^CProcessed a total of 2 messages
```


# Once the broker 2 and 3 setup done, you should see three brokers here:
```
bin/zookeeper-shell.sh localhost:2181
ls /kafka/brokers/ids
```

> Results:
```
ubuntu@kafka2:~/kafka$ bin/zookeeper-shell.sh localhost:2181
Connecting to localhost:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

ls /kafka/brokers/ids
[1, 2, 3]
```

Next: [Test Kafka cluster](8-test-kafka-cluster.md)
