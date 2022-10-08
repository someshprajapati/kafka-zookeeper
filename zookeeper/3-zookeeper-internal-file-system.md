### ZooKeeper Internal file systems

`cd /data/zookeeper/`

> Results:
```
ubuntu@kafka1:/data/zookeeper$ cd /data/zookeeper/

ubuntu@kafka1:/data/zookeeper$ ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu    2 Oct  8 12:04 myid
drwxr-xr-x 2 root   root   4096 Oct  8 12:11 version-2

ubuntu@kafka1:/data/zookeeper$ cat myid
1

ubuntu@kafka1:/data/zookeeper$ ls -l version-2/
total 12
-rw-r--r-- 1 root root   1 Oct  8 12:11 acceptedEpoch
-rw-r--r-- 1 root root   1 Oct  8 12:11 currentEpoch
-rw-r--r-- 1 root root 296 Oct  8 12:11 snapshot.0
```
