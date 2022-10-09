## Make sure you open port 9000 on the security group

### Make sure you can access the zookeeper endpoints from kafka-manager server
```
nc -vz zookeeper1 2181
nc -vz zookeeper2 2181
nc -vz zookeeper3 2181
```

> Results:
```
ubuntu@kafka-manager:~$ nc -vz zookeeper1 2181
Connection to zookeeper1 2181 port [tcp/*] succeeded!

ubuntu@kafka-manager:~$ nc -vz zookeeper2 2181
Connection to zookeeper2 2181 port [tcp/*] succeeded!

ubuntu@kafka-manager:~$ nc -vz zookeeper3 2181
Connection to zookeeper3 2181 port [tcp/*] succeeded!
```

### Make sure you can access the kafka endpoints from kafka-manager server
```
nc -vz kafka1 9092
nc -vz kafka2 9092
nc -vz kafka3 9092
```

> Results:
```
ubuntu@kafka-manager:~$ nc -vz kafka1 9092
Connection to kafka1 9092 port [tcp/*] succeeded!

ubuntu@kafka-manager:~$ nc -vz kafka2 9092
Connection to kafka2 9092 port [tcp/*] succeeded!

ubuntu@kafka-manager:~$ nc -vz kafka3 9092
Connection to kafka3 9092 port [tcp/*] succeeded!
```

### Copy the `kafka-manager-docker-compose.yml` file from tools folder
`vim kafka-manager-docker-compose.yml`

### Launch it
`docker-compose -f kafka-manager-docker-compose.yml up -d`

> Results:
```
ubuntu@kafka-manager:~$ docker-compose -f kafka-manager-docker-compose.yml up -d
Pulling kafka-manager (qnib/plain-kafka-manager:latest)...
latest: Pulling from qnib/plain-kafka-manager
8e3ba11ec2a2: Pull complete
4a7048585c9d: Pull complete
edf021a9591a: Pull complete
3b476c870fa9: Pull complete
da3692b0b8dd: Pull complete
ae3fd1b7d5ce: Pull complete
5ee46015cb47: Pull complete
a3b9c9facbf3: Pull complete
019fd85f3bc0: Pull complete
da38bd018f0d: Pull complete
b8122b7b98d2: Pull complete
65430c0ddb28: Pull complete
8ef27066081d: Pull complete
Digest: sha256:ab23b403732dcb92938e586fa42f5eedf88761871a713fea89996755d0682e0d
Status: Downloaded newer image for qnib/plain-kafka-manager:latest
Creating ubuntu_kafka-manager_1 ...
Creating ubuntu_kafka-manager_1 ... done


ubuntu@kafka-manager:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS                    PORTS                                       NAMES
a9c43fbde915   qnib/plain-kafka-manager   "/usr/local/bin/entr…"   31 minutes ago   Up 5 minutes (healthy)   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp   ubuntu_kafka-manager_1

ubuntu@kafka-manager:~$ netstat -tlpn
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:9000            0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::9000                 :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
```

### Open Kafka Manager in browser:
`http://192.168.233.135:9000/`


### Fix the red error showed in [Topic List](images/3-Topic List.png) using below:

> Results:
```
ubuntu@kafka2:~/kafka$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092 --topic second_topic --from-beginning
Hi
it's working
Welcome back
Kafka setup testing
^CProcessed a total of 4 messages
```

> Image Links:

[Kafka Manager UI](0-Kafka Manager UI.png)

[Add Cluster](1-Add Cluster.png)

[Cluster Info](2-Cluster Info.png)

[Topic List](3-Topic List.png)

[Topic View](4-Topic View.png)

[Create Topic from UI](5-Create Topic from UI.png)


Next: [Kafka Manager Resiliency](10-test-kafka-resiliency.md)
