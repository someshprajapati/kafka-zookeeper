### Let's apply a new setting to our server.properties
`pwd`

### Make sure you're in the /home/ubuntu/kafka directory
```
cat config/server.properties
echo "unclean.leader.election.enable=false" >> config/server.properties
cat config/server.properties
```

### Look at the logs - what was the value before?
`cat logs/server.log | grep unclean.leader`

### Stop the broker
`sudo service kafka stop`

### Restart the broker
`sudo service kafka start`

### look at the logs - what is the value after?
`cat logs/server.log | grep unclean.leader`

### operate on the three brokers

Next: [Kafka Topics UI](12-kafka-topics-ui.md)
