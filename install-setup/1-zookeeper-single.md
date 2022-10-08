### System Info
```
ubuntu@kafka1:~$ uname -a
Linux kafka1 4.15.0-193-generic #204-Ubuntu SMP Fri Aug 26 19:20:21 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

ubuntu@kafka2:~$ uname -a
Linux kafka2 4.15.0-193-generic #204-Ubuntu SMP Fri Aug 26 19:20:21 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

ubuntu@kafka3:~$ uname -a
Linux kafka3 4.15.0-193-generic #204-Ubuntu SMP Fri Aug 26 19:20:21 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

### Install basic Packages
`sudo apt-get update`

`sudo apt-get -y install wget ca-certificates zip net-tools vim nano tar netcat`

### Java Open JDK 8
`sudo apt-get -y install openjdk-8-jdk`

`java -version`

> Results:
```
ubuntu@kafka1:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)

ubuntu@kafka2:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)

ubuntu@kafka3:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)
```


### Disable RAM Swap - can set to 0 on certain Linux distro
`sudo sysctl vm.swappiness=1`

`echo 'vm.swappiness=1' | sudo tee --append /etc/sysctl.conf`

### Add hosts entries (mocking DNS) - put relevant IPs here
```
echo "192.168.233.131 kafka1
192.168.233.131 zookeeper1
192.168.233.132 kafka2
192.168.233.132 zookeeper2
192.168.233.133 kafka3
192.168.233.133 zookeeper3" | sudo tee --append /etc/hosts
```

### Download Zookeeper and Kafka. Recommended is latest Kafka (0.10.2.1)
```
wget https://archive.apache.org/dist/kafka/0.10.2.1/kafka_2.12-0.10.2.1.tgz
tar -xvzf kafka_2.12-0.10.2.1.tgz
rm kafka_2.12-0.10.2.1.tgz
mv kafka_2.12-0.10.2.1 kafka
cd kafka/
```

### Zookeeper quickstart
`cat config/zookeeper.properties`

`bin/zookeeper-server-start.sh config/zookeeper.properties`

NOTE: binding to port 2181 -> you're good. Ctrl+C to exit

## Testing Zookeeper install
### Start Zookeeper in the background
`bin/zookeeper-server-start.sh -daemon config/zookeeper.properties`
`bin/zookeeper-shell.sh localhost:2181`
`ls /`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
ubuntu@kafka1:~/kafka$ bin/zookeeper-shell.sh localhost:2181
Connecting to localhost:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
ls /
[zookeeper]
```

### Demonstrate the use of a 4 letter word
`echo "ruok" | nc localhost 2181 ; echo`
> Results:
imok

### Install Zookeeper boot scripts

Copy from the file `kafka-zookeeper/zookeeper/zookeeper`
```
sudo vim /etc/init.d/zookeeper
sudo chmod +x /etc/init.d/zookeeper
sudo chown root:root /etc/init.d/zookeeper
```

### Safely ignore the warning if any
`sudo update-rc.d zookeeper defaults`

### Stop zookeeper
`sudo service zookeeper stop`

### Verify it's stopped
`nc -vz localhost 2181`

> Results:
```
ubuntu@kafka1:~/kafka$ sudo service zookeeper status
● zookeeper.service - SYSV: Standard script to start and stop zookeeper
   Loaded: loaded (/etc/init.d/zookeeper; generated)
   Active: inactive (dead)
     Docs: man:systemd-sysv-generator(8)

ubuntu@kafka1:~/kafka$ nc -vz localhost 2181
nc: connect to localhost port 2181 (tcp) failed: Connection refused

ubuntu@kafka1:~/kafka$ echo "ruok" | nc localhost 2181 ; echo
```

### Start zookeeper
`sudo service zookeeper start`

### Verify it's started
`nc -vz localhost 2181`
`echo "ruok" | nc localhost 2181 ; echo`

> Results:
```
ubuntu@kafka1:~/kafka$ sudo service zookeeper start

ubuntu@kafka1:~/kafka$ sudo service zookeeper status
● zookeeper.service - SYSV: Standard script to start and stop zookeeper
   Loaded: loaded (/etc/init.d/zookeeper; generated)
   Active: active (running) since Sat 2022-10-08 10:53:05 UTC; 3s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 9447 ExecStart=/etc/init.d/zookeeper start (code=exited, status=0/SUCCESS)
    Tasks: 23 (limit: 4625)
   CGroup: /system.slice/zookeeper.service
           └─9687 java -Xmx512M -Xms512M -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+DisableExplicitGC -Djava.awt.headless=true -Xlogg

Oct 08 10:53:04 kafka1 systemd[1]: Starting SYSV: Standard script to start and stop zookeeper...
Oct 08 10:53:04 kafka1 zookeeper[9447]: Starting zookeeper
Oct 08 10:53:05 kafka1 systemd[1]: Started SYSV: Standard script to start and stop zookeeper.

ubuntu@kafka1:~/kafka$ nc -vz localhost 2181
Connection to localhost 2181 port [tcp/*] succeeded!

ubuntu@kafka1:~/kafka$ echo "ruok" | nc localhost 2181 ; echo
imok
```

### Check the logs
`cat logs/zookeeper.out`

> Results:
```
ubuntu@kafka1:~/kafka$ cat logs/zookeeper.out
[2022-10-08 10:53:05,411] INFO Reading configuration from: /home/ubuntu/kafka/config/zookeeper.properties (org.apache.zookeeper.server.quorum.QuorumPeerConfig)
.
.
.
[2022-10-08 10:53:21,254] INFO Accepted socket connection from /127.0.0.1:37904 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2022-10-08 10:53:21,255] INFO Processing ruok command from /127.0.0.1:37904 (org.apache.zookeeper.server.NIOServerCnxn)
[2022-10-08 10:53:21,258] INFO Closed socket connection for client /127.0.0.1:37904 (no session established for client) (org.apache.zookeeper.server.NIOServerCnxn)
```
