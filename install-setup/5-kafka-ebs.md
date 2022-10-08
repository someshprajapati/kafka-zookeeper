### Execute commands as root
`sudo su`

### Attach the EBS volume in the console, then view available disks
`lsblk`

### We verify the disk is empty - should return "data"
`file -s /dev/xvdf`

### Note on Kafka: it's better to format volumes as xfs:
#### https://kafka.apache.org/documentation/#filesystems
#### Install packages to mount as xfs
`apt-get install -y xfsprogs`

### Create a partition
`fdisk /dev/xvdf`

### Format as xfs
`mkfs.xfs -f /dev/xvdf`

### Create kafka directory
`mkdir /data/kafka`

### Mount volume
`mount -t xfs /dev/xvdf /data/kafka`

### Add permissions to kafka directory
`chown -R ubuntu:ubuntu /data/kafka`

### Check it's working
`df -h /data/kafka`

### EBS Automount On Reboot
`cp /etc/fstab /etc/fstab.bak`

`echo '/dev/xvdf /data/kafka xfs defaults 0 0' >> /etc/fstab`

### Reboot to test actions
`reboot`

`sudo service zookeeper start`

Next: [Kafka Single Node](6-kafka-single.md)
