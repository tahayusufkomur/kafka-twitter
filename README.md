# kafka-twitter
In this project, i will use twitter developer API, kafka and Java to create real time data flowing project.
In every commit there will be steps i followed



### Commit 1

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
	- move  hosebirdClient.connect(); to run function
	-  move MSQQUE to run()
	- pass msgqueue as an argument to twitterClient()
	- change msg.Queue.take to .poll and add try catch
	  
- Create a kafka producer
	- create Logger inside TwitterProducer()
	
- loop to send tweets to kafka
	- copy while loop under run()
