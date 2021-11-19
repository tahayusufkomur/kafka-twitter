# kafka-twitter
In this project, i will use twitter developer API, kafka and Java to create real time data flowing project.
In every commit there will be steps i followed



### STEPS

TWITTER AUTHENTICATION:

```
String consumerKey = "**************";
String consumerSecret = "************";
String token = "***************";
String tokenSecret = "***********";
```


1- New Project
- New Project
- Maven Project
- Name: kafka-twitter
- Go under directories, right click java, create new package
- com.github.kafka.twitter
- new class under the package "TwitterProducer"
- 
2- Creating Kafka Producer

-  Add Dependencies
	- go to https://mvnrepository.com/artifact/org.apache.kafka
	- copy maven dependency to pom.xml
	- go to https://mvnrepository.com/artifact/org.slf4j/slf4j-simple/1.7.25
	- copy maven dependency to pom.xml
	- go to https://github.com/twitter/hbc
	- copy maven dependency to pom.xml
	  
- Create a twitter client
	- createTwitterClient() method
	- go to https://github.com/twitter/hbc
	- copy BlockingQueue msgqueue to createTwitterClient()
	- copy hosebird and other stuffs
	- add authentication strings to Authentication
	- go to https://github.com/twitter/hbc
	- copy ClientBuilder builder
	- change function in order to return hosebirdClient;
	- move  hosebirdClient.connect(); to run function
	-  move MSQQUE to run()
	- pass msgqueue as an argument to twitterClient()
	- change msg.Queue.take to .poll and add try catch
	  
- Create a kafka producer
	- create Logger inside TwitterProducer()
	- create bootstrap servers
	- create properties
	- config (bootstrapserver,keySerializer,valueSerializer)
	- return producer created with properties.
	- add a shutdown hook
	
- loop to send tweets to kafka
	- copy while loop under run()
	- add producer.send() with topic, key and value, but Callback() it.
	- create topic on kafka
		- zookeeper-server-start.sh /Users/duma/kafka_2.13-3.0.0/config/zookeeper.properties
		- kafka-server-start.sh /Users/duma/kafka_2.13-3.0.0/config/server.properties
		- kafka-topics.sh --bootstrap-server localhost:9092 --create --topic twitter_topic --partitions 6 --replication-factor 1
		- kafka-topics.sh --list --bootstrap-server localhost:9092
		- IMPORTANT 9092 is bootstrap-servers port number.
- Producer configuration
	- ( if kafka<0.11 ) this settings are important
	- ack: waiting for response of brokers
		- acks = 0 ( no acks) (not safe)
		- acks = 1 ( response from leader broker)
		- acks = 2 (leader broker get all ack from all brokers) (more latency)
		- acks = 2(all) must be used in conjunction with min.insync.replicas, 
			- ex: if you use rep_fac 3, minsync 2, acks=all then you can tolerate 1 broker going down
	- retries: in case of failures
		- retry = 0 ( no try in case of failure )
		- retry = 2147483647 ( will try for very long time )
		- retry.backoff.ms setting is by default 100ms
		- but still producer timeout bounds retry, by default 120 000 ms, 2 minutes.
	-   If you need ordering to be ensured, you need to set max.in.flight.requests.per.connection to 1, it limits parallel request for brokers. Its 5 default. 
	  
	- Idemponent Producer (BETTER SOLUTION ) 
		- new thing with kafka 0.11+, produce requests has id, producer don't commit twice, sends ack again if had data.
		- IT COME WITH:
			- retries = Integer.MAX_VALUE(2^31-1=2147483647)
			- max.in.flight.requests=1 for kafka == 0.11 or
			- max.in.flight.requests=5 for kafka 1.0+ (higher performance, keep ordering)
		- just set:
			- producerProps.put("enable.idempotence",true);
- Message Compression
	- compression.type can be 'none', 'gzip', 'lz4', 'snappy'
	- Compression is enabled in Producer level and does not require any configuration change in the Brokers or in the Consumers.

3- Creating Kafka Consumer

- Create properties,  logger, kafka consumer
- Create while loop and records = consumer.poll
