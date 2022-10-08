### Install packages to allow apt to use a repository over HTTPS:
`sudo apt-get update`

`sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common`

### Add Docker’s official GPG key:
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

### Set up the stable repository.
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### Install docker
`sudo apt-get update`

`sudo apt-get install -y docker-ce docker-compose`

### Give ubuntu permissions to execute docker
`sudo usermod -aG docker $(whoami)`

### Logout
`exit`

### Log back in and make sure docker is working
`docker run hello-world`

### Add hosts entries (mocking DNS) - put relevant IPs here
```
echo "192.168.233.131 kafka1
192.168.233.131 zookeeper1
192.168.233.132 kafka2
192.168.233.132 zookeeper2
192.168.233.133 kafka3
192.168.233.133 zookeeper3" | sudo tee --append /etc/hosts
```

Next: [Zoonavigator](4-zoonavigator.md)
