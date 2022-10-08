### Include zookeeper-shell examples
`bin/zookeeper-shell.sh localhost:2181`

> Results:
```
ubuntu@kafka1:~/kafka$ bin/zookeeper-shell.sh localhost:2181
Connecting to localhost:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
```

### Display help
`help`

> Results:
```
help
ZooKeeper -server host:port cmd args
	stat path [watch]
	set path data [version]
	ls path [watch]
	delquota [-n|-b] path
	ls2 path [watch]
	setAcl path acl
	setquota -n|-b val path
	history
	redo cmdno
	printwatches on|off
	delete path [version]
	sync path
	listquota path
	rmr path
	get path [watch]
	create [-s] [-e] path data acl
	addauth scheme auth
	quit
	getAcl path
	close
	connect host:port
```

### Display root and create node

```
ls /

create /my-first-node "somesh"

ls /

get /my-first-node
get /zookeeper

create /my-first-node/deeper-node "somesh-level-1"
ls /
ls /my-first-node
get /my-first-node
get /my-first-node/deeper-node
```

> Results:
```
ls /
[zookeeper]

create /my-first-node "somesh"
Created /my-first-node

ls /
[my-first-node, zookeeper]

ls /my-first-node
[]

ls /zookeeper
[quota]

create /my-first-node/deeper-node "somesh-level-1"
Created /my-first-node/deeper-node

ls /
[my-first-node, zookeeper]

ls /my-first-node
[deeper-node]

get /my-first-node
somesh
cZxid = 0x6
ctime = Sat Oct 08 11:23:26 UTC 2022
mZxid = 0x6
mtime = Sat Oct 08 11:23:26 UTC 2022
pZxid = 0x7
cversion = 1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 6
numChildren = 1

get /my-first-node/deeper-node
somesh-level-1
cZxid = 0x7
ctime = Sat Oct 08 11:36:01 UTC 2022
mZxid = 0x7
mtime = Sat Oct 08 11:36:01 UTC 2022
pZxid = 0x7
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 14
numChildren = 0
```

### Update data version to see increased dataVersion counter
`set /my-first-node/deeper-node "newdata"`
`get /my-first-node/deeper-node`

> Results:
```
set /my-first-node/deeper-node "newdata"
cZxid = 0x7
ctime = Sat Oct 08 11:36:01 UTC 2022
mZxid = 0x8
mtime = Sat Oct 08 11:41:32 UTC 2022
pZxid = 0x7
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0

get /my-first-node/deeper-node
newdata
cZxid = 0x7
ctime = Sat Oct 08 11:36:01 UTC 2022
mZxid = 0x8
mtime = Sat Oct 08 11:41:32 UTC 2022
pZxid = 0x7
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0
```

### Removes are recursive
`rmr /my-first-node`
`ls /`

> Results:
```
ls /
[my-first-node, zookeeper]

ls /my-first-node
[deeper-node]

rmr /my-first-node

ls /
[zookeeper]
```

### Create a watcher
```
create /node-to-watch ""
get /node-to-watch true
set /node-to-watch "has-changed"
rmr /node-to-watch
```

> Results:
```
create /node-to-watch ""
Created /node-to-watch

get /node-to-watch true

cZxid = 0xb
ctime = Sat Oct 08 11:45:45 UTC 2022
mZxid = 0xb
mtime = Sat Oct 08 11:45:45 UTC 2022
pZxid = 0xb
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 0

set /node-to-watch "has-changed"

WATCHER::

WatchedEvent state:SyncConnected type:NodeDataChanged path:/node-to-watch
cZxid = 0xb
ctime = Sat Oct 08 11:45:45 UTC 2022
mZxid = 0xc
mtime = Sat Oct 08 11:46:46 UTC 2022
pZxid = 0xb
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 11
numChildren = 0

rmr /node-to-watch
```
