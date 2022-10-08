# ZooKeeper setup on Kafka2 and Kafka3 servers
Follow the same steps on kafka2 and kafka3 servers which we followed on kafka1 server previously: [zookeeper-single](1-zookeeper-single.md)

### Basic connection testing before start the Quorum
> Kafka1:
```
ubuntu@kafka1:~/kafka$ nc -vz localhost 2181
Connection to localhost 2181 port [tcp/*] succeeded!

ubuntu@kafka1:~/kafka$ nc -vz zookeeper1 2181
Connection to zookeeper1 2181 port [tcp/*] succeeded!

ubuntu@kafka1:~/kafka$ nc -vz zookeeper2 2181
Connection to zookeeper2 2181 port [tcp/*] succeeded!

ubuntu@kafka1:~/kafka$ nc -vz zookeeper3 2181
Connection to zookeeper3 2181 port [tcp/*] succeeded!
```

> Kafka2:
```
ubuntu@kafka2:~/kafka$ nc -vz localhost 2181
Connection to localhost 2181 port [tcp/*] succeeded!

ubuntu@kafka2:~/kafka$ nc -vz zookeeper1 2181
Connection to zookeeper1 2181 port [tcp/*] succeeded!

ubuntu@kafka2:~/kafka$ nc -vz zookeeper2 2181
Connection to zookeeper2 2181 port [tcp/*] succeeded!

ubuntu@kafka2:~/kafka$ nc -vz zookeeper3 2181
Connection to zookeeper3 2181 port [tcp/*] succeeded!
```

> Kafka3:
```
ubuntu@kafka3:~/kafka$ nc -vz localhost 2181
Connection to localhost 2181 port [tcp/*] succeeded!

ubuntu@kafka3:~/kafka$ nc -vz zookeeper1 2181
Connection to zookeeper1 2181 port [tcp/*] succeeded!

ubuntu@kafka3:~/kafka$ nc -vz zookeeper2 2181
Connection to zookeeper2 2181 port [tcp/*] succeeded!

ubuntu@kafka3:~/kafka$ nc -vz zookeeper3 2181
Connection to zookeeper3 2181 port [tcp/*] succeeded!
```

### Create data dictionary for zookeeper in all servers
`sudo mkdir -p /data/zookeeper`

`sudo chown -R ubuntu:ubuntu /data/`

### Declare the server's identity
`echo "1" > /data/zookeeper/myid`

> Results:
```
ubuntu@kafka1:~/kafka$ echo "1" > /data/zookeeper/myid
ubuntu@kafka1:~/kafka$ cat /data/zookeeper/myid
1

ubuntu@kafka2:~/kafka$ echo "2" > /data/zookeeper/myid
ubuntu@kafka2:~/kafka$ cat /data/zookeeper/myid
2

ubuntu@kafka3:~/kafka$ echo "3" > /data/zookeeper/myid
ubuntu@kafka3:~/kafka$ cat /data/zookeeper/myid
3
```

### Edit the zookeeper settings (copy the file from the `zookeeper/zookeeper.properties`)
`rm /home/ubuntu/kafka/config/zookeeper.properties`

`vim /home/ubuntu/kafka/config/zookeeper.properties`

> Results:
```
ubuntu@kafka1:~/kafka$ cat /home/ubuntu/kafka/config/zookeeper.properties
# the location to store the in-memory database snapshots and, unless specified otherwise, the transaction log of updates to the database.
dataDir=/data/zookeeper
# the port at which the clients will connect
clientPort=2181
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=0
# the basic time unit in milliseconds used by ZooKeeper. It is used to do heartbeats and the minimum session timeout will be twice the tickTime.
tickTime=2000
# The number of ticks that the initial synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# zoo servers
# these hostnames such as `zookeeper-1` come from the /etc/hosts file
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

### Restart the zookeeper service
`sudo service zookeeper stop`

`sudo service zookeeper start`

### Verify if the Quorum is working, check on any server
`echo "ruok" | nc zookeeper1 2181 ; echo`

`echo "stat" | nc zookeeper1 2181 ; echo`

> Results:
```
ubuntu@kafka1:~/kafka$ echo "ruok" | nc zookeeper1 2181 ; echo
imok

ubuntu@kafka1:~/kafka$ echo "ruok" | nc zookeeper2 2181 ; echo
imok

ubuntu@kafka1:~/kafka$ echo "ruok" | nc zookeeper3 2181 ; echo
imok
```

> Stats Results:
```
ubuntu@kafka1:~/kafka$ echo "stat" | nc zookeeper1 2181 ; echo
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /192.168.233.131:39822[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 4
Sent: 3
Connections: 1
Outstanding: 0
Zxid: 0x0
Mode: follower
Node count: 4


ubuntu@kafka1:~/kafka$ echo "stat" | nc zookeeper2 2181 ; echo
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /192.168.233.131:39316[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 6
Sent: 5
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: leader
Node count: 4


ubuntu@kafka1:~/kafka$ echo "stat" | nc zookeeper3 2181 ; echo
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /192.168.233.131:44342[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 7
Sent: 6
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: follower
Node count: 4
```

### Observe the logs - need to do this on every machine
```
cat /home/ubuntu/kafka/logs/zookeeper.out | head -100
nc -vz localhost 2181
nc -vz localhost 2888
nc -vz localhost 3888
echo "ruok" | nc zookeeper1 2181 ; echo
echo "stat" | nc zookeeper1 2181 ; echo

bin/zookeeper-shell.sh zookeeper1:2181
bin/zookeeper-shell.sh zookeeper2:2181
bin/zookeeper-shell.sh zookeeper3:2181
```

Next: [Four-letter-word-commands](2-four-letter-word-commands.md)
