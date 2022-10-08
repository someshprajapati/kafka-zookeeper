### Refrences: 
https://zookeeper.apache.org/doc/r3.4.8/zookeeperAdmin.html#sc_zkCommands

## conf
#### New in 3.3.0: Print details about serving configuration.
`echo "conf" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "conf" | nc localhost 2181
clientPort=2181
dataDir=/data/zookeeper/version-2
dataLogDir=/data/zookeeper/version-2
tickTime=2000
maxClientCnxns=0
minSessionTimeout=4000
maxSessionTimeout=40000
serverId=1
initLimit=10
syncLimit=5
electionAlg=3
electionPort=3888
quorumPort=2888
peerType=0

ubuntu@kafka2:~/kafka$ echo "conf" | nc localhost 2181
clientPort=2181
dataDir=/data/zookeeper/version-2
dataLogDir=/data/zookeeper/version-2
tickTime=2000
maxClientCnxns=0
minSessionTimeout=4000
maxSessionTimeout=40000
serverId=2
initLimit=10
syncLimit=5
electionAlg=3
electionPort=3888
quorumPort=2888
peerType=0

ubuntu@kafka3:~/kafka$ echo "conf" | nc localhost 2181
clientPort=2181
dataDir=/data/zookeeper/version-2
dataLogDir=/data/zookeeper/version-2
tickTime=2000
maxClientCnxns=0
minSessionTimeout=4000
maxSessionTimeout=40000
serverId=3
initLimit=10
syncLimit=5
electionAlg=3
electionPort=3888
quorumPort=2888
peerType=0
```

## cons
#### New in 3.3.0: List full connection/session details for all clients connected to this server. Includes information on numbers of packets received/sent, session id, operation latencies, last operation performed, etc...
`echo "cons" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "cons" | nc localhost 2181
 /127.0.0.1:39474[0](queued=0,recved=1,sent=0)

ubuntu@kafka2:~/kafka$ echo "cons" | nc localhost 2181
 /127.0.0.1:49598[0](queued=0,recved=1,sent=0)

ubuntu@kafka3:~/kafka$ echo "cons" | nc localhost 2181
 /127.0.0.1:41340[0](queued=0,recved=1,sent=0)
```

## crst
#### New in 3.3.0: Reset connection/session statistics for all connections.
`echo "crst" | nc localhost 2181`

## srst
#### Reset server statistics
`echo "srst" | nc localhost 2181`

> Results:

```
Server stats reset.
```

## dump
#### Lists the outstanding sessions and ephemeral nodes. This only works on the leader.
`echo "dump" | nc localhost 2181`

## envi
#### Print details about serving environment
`echo "envi" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "envi" | nc localhost 2181
Environment:
zookeeper.version=3.4.9-1757313, built on 08/23/2016 06:50 GMT
host.name=kafka1
java.version=1.8.0_342
java.vendor=Private Build
java.home=/usr/lib/jvm/java-8-openjdk-amd64/jre
java.class.path=:/home/ubuntu/kafka/bin/../libs/aopalliance-repackaged-2.5.0-b05.jar:/home/ubuntu/kafka/bin/../libs/argparse4j-0.7.0.jar:/home/ubuntu/kafka/bin/../libs/connect-api-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/connect-file-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/connect-json-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/connect-runtime-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/connect-transforms-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/guava-18.0.jar:/home/ubuntu/kafka/bin/../libs/hk2-api-2.5.0-b05.jar:/home/ubuntu/kafka/bin/../libs/hk2-locator-2.5.0-b05.jar:/home/ubuntu/kafka/bin/../libs/hk2-utils-2.5.0-b05.jar:/home/ubuntu/kafka/bin/../libs/jackson-annotations-2.8.0.jar:/home/ubuntu/kafka/bin/../libs/jackson-annotations-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/jackson-core-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/jackson-databind-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/jackson-jaxrs-base-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/jackson-jaxrs-json-provider-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/jackson-module-jaxb-annotations-2.8.5.jar:/home/ubuntu/kafka/bin/../libs/javassist-3.20.0-GA.jar:/home/ubuntu/kafka/bin/../libs/javax.annotation-api-1.2.jar:/home/ubuntu/kafka/bin/../libs/javax.inject-1.jar:/home/ubuntu/kafka/bin/../libs/javax.inject-2.5.0-b05.jar:/home/ubuntu/kafka/bin/../libs/javax.servlet-api-3.1.0.jar:/home/ubuntu/kafka/bin/../libs/javax.ws.rs-api-2.0.1.jar:/home/ubuntu/kafka/bin/../libs/jersey-client-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-common-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-container-servlet-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-container-servlet-core-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-guava-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-media-jaxb-2.24.jar:/home/ubuntu/kafka/bin/../libs/jersey-server-2.24.jar:/home/ubuntu/kafka/bin/../libs/jetty-continuation-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-http-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-io-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-security-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-server-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-servlet-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-servlets-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jetty-util-9.2.15.v20160210.jar:/home/ubuntu/kafka/bin/../libs/jopt-simple-5.0.3.jar:/home/ubuntu/kafka/bin/../libs/kafka_2.12-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/kafka_2.12-0.10.2.1-sources.jar:/home/ubuntu/kafka/bin/../libs/kafka_2.12-0.10.2.1-test-sources.jar:/home/ubuntu/kafka/bin/../libs/kafka-clients-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/kafka-log4j-appender-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/kafka-streams-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/kafka-streams-examples-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/kafka-tools-0.10.2.1.jar:/home/ubuntu/kafka/bin/../libs/log4j-1.2.17.jar:/home/ubuntu/kafka/bin/../libs/lz4-1.3.0.jar:/home/ubuntu/kafka/bin/../libs/metrics-core-2.2.0.jar:/home/ubuntu/kafka/bin/../libs/osgi-resource-locator-1.0.1.jar:/home/ubuntu/kafka/bin/../libs/reflections-0.9.10.jar:/home/ubuntu/kafka/bin/../libs/rocksdbjni-5.0.1.jar:/home/ubuntu/kafka/bin/../libs/scala-library-2.12.1.jar:/home/ubuntu/kafka/bin/../libs/scala-parser-combinators_2.12-1.0.4.jar:/home/ubuntu/kafka/bin/../libs/slf4j-api-1.7.21.jar:/home/ubuntu/kafka/bin/../libs/slf4j-log4j12-1.7.21.jar:/home/ubuntu/kafka/bin/../libs/snappy-java-1.1.2.6.jar:/home/ubuntu/kafka/bin/../libs/validation-api-1.1.0.Final.jar:/home/ubuntu/kafka/bin/../libs/zkclient-0.10.jar:/home/ubuntu/kafka/bin/../libs/zookeeper-3.4.9.jar
java.library.path=/usr/java/packages/lib/amd64:/usr/lib/x86_64-linux-gnu/jni:/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu:/usr/lib/jni:/lib:/usr/lib
java.io.tmpdir=/tmp
java.compiler=<NA>
os.name=Linux
os.arch=amd64
os.version=4.15.0-193-generic
user.name=root
user.home=/root
user.dir=/
```

## ruok
#### Tests if server is running in a non-error state. The server will respond with imok if it is running. Otherwise it will not respond at all.
`echo "ruok" | nc localhost 2181`

> NOTE: A response of "imok" does not necessarily indicate that the server has joined the quorum, just that the server process is active and bound to the specified client port. Use "stat" for details on state wrt quorum and client connection information.

## srvr
#### New in 3.3.0: Lists full details for the server.
`echo "srvr" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "srvr" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Latency min/avg/max: 0/0/0
Received: 1
Sent: 1
Connections: 1
Outstanding: 0
Zxid: 0x0
Mode: follower
Node count: 4


ubuntu@kafka2:~/kafka$ echo "srvr" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Latency min/avg/max: 0/0/0
Received: 1
Sent: 1
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: leader
Node count: 4


ubuntu@kafka3:~/kafka$ echo "srvr" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Latency min/avg/max: 0/0/0
Received: 1
Sent: 1
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: follower
Node count: 4
```

## stat
#### Lists brief details for the server and connected clients.
`echo "stat" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "stat" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /127.0.0.1:57334[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 2
Sent: 2
Connections: 1
Outstanding: 0
Zxid: 0x0
Mode: follower
Node count: 4


ubuntu@kafka2:~/kafka$ echo "stat" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /127.0.0.1:46230[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 2
Sent: 2
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: leader
Node count: 4


ubuntu@kafka3:~/kafka$ echo "stat" | nc localhost 2181
Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
Clients:
 /127.0.0.1:45548[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 2
Sent: 2
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: follower
Node count: 4
```


## wchs
#### New in 3.3.0: Lists brief information on watches for the server.
`echo "wchs" | nc localhost 2181`

> Results:

```
ubuntu@kafka1:~/kafka$ echo "wchs" | nc localhost 2181
0 connections watching 0 paths
Total watches:0

ubuntu@kafka2:~/kafka$ echo "wchs" | nc localhost 2181
0 connections watching 0 paths
Total watches:0

ubuntu@kafka3:~/kafka$ echo "wchs" | nc localhost 2181
0 connections watching 0 paths
Total watches:0
```

## wchc
#### New in 3.3.0: Lists detailed information on watches for the server, by session. This outputs a list of sessions(connections) with associated watches (paths). Note, depending on the number of watches this operation may be expensive (ie impact server performance), use it carefully.
`echo "wchc" | nc localhost 2181`


## wchp
#### New in 3.3.0: Lists detailed information on watches for the server, by path. This outputs a list of paths (znodes) with associated sessions. Note, depending on the number of watches this operation may be expensive (ie impact server performance), use it carefully.
`echo "wchp" | nc localhost 2181`


## mntr
#### New in 3.4.0: Outputs a list of variables that could be used for monitoring the health of the cluster.
echo "mntr" | nc localhost 2181

> Results:

```
ubuntu@kafka1:~/kafka$ echo "mntr" | nc localhost 2181
zk_version	3.4.9-1757313, built on 08/23/2016 06:50 GMT
zk_avg_latency	0
zk_max_latency	0
zk_min_latency	0
zk_packets_received	6
zk_packets_sent	6
zk_num_alive_connections	1
zk_outstanding_requests	0
zk_server_state	follower
zk_znode_count	4
zk_watch_count	0
zk_ephemerals_count	0
zk_approximate_data_size	27
zk_open_file_descriptor_count	104
zk_max_file_descriptor_count	4096


ubuntu@kafka2:~/kafka$ echo "mntr" | nc localhost 2181
zk_version	3.4.9-1757313, built on 08/23/2016 06:50 GMT
zk_avg_latency	0
zk_max_latency	0
zk_min_latency	0
zk_packets_received	6
zk_packets_sent	6
zk_num_alive_connections	1
zk_outstanding_requests	0
zk_server_state	leader
zk_znode_count	4
zk_watch_count	0
zk_ephemerals_count	0
zk_approximate_data_size	27
zk_open_file_descriptor_count	106
zk_max_file_descriptor_count	4096
zk_followers	2
zk_synced_followers	2
zk_pending_syncs	0


ubuntu@kafka3:~/kafka$ echo "mntr" | nc localhost 2181
zk_version	3.4.9-1757313, built on 08/23/2016 06:50 GMT
zk_avg_latency	0
zk_max_latency	0
zk_min_latency	0
zk_packets_received	6
zk_packets_sent	6
zk_num_alive_connections	1
zk_outstanding_requests	0
zk_server_state	follower
zk_znode_count	4
zk_watch_count	0
zk_ephemerals_count	0
zk_approximate_data_size	27
zk_open_file_descriptor_count	104
zk_max_file_descriptor_count	4096
```

Next: [Zookeeper-internal-file-system](3-zookeeper-internal-file-system.md)