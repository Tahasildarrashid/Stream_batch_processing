# STREAM-BATCH-PROCESSING-KAFKA-SPARK

## SYNOPSIS
- A project which includes simulation of ```real time``` queries by ```kafka``` and performing ```stream and batch processing``` of the simulated queries by spark. 
- Also, this follows ```lambda architecture```, in which kafka is publisher and spark helps in subscribing. 
- Here we are simulating the real time data instead of using any real-time API's. This is achieved by ```randomnized streaming``` of data from the database.

## PROBLEM DESCRIPTION AND INTRODUCTION
- The main challenge addressed in this project is to process the massive volume of data generated by Twitter and perform various actions, transformations, and aggregations on it. 
- To address this challenge, the project involves creating multiple topics using Kafka, such as ```tweets, hashtags, top_tweets, and top_hashtags```, and continuously publishing data from the MySQL database to these topics. 
- Also, we can be assured that kafka does the ```real-time streaming``` of data from the database, as mentioned in the architecture below. 
- This simulation is near to the ```Twitter API like performance```. Overall, this project aims to demonstrate how Apache Spark and Kafka can be used to efficiently process and analyse real-time streaming data, with a focus on Twitter feeds and gain valuable insights. 
- We also see the batch processing of the queries, which is also a major use of Spark. We can also understand that Spark does the subscribing part of the entire model. - At last, we see the comparison of a topic between the stream and batch processing and application of the type of windows, etc… 

## TECHNOLOGIES / FRAMEWORKS TO BE EXERCISED:
- Ubuntu 20.04 or 22.04 or any other LINUX environment
- Zookeeper
- Apache Spark Streaming, or pyspark
```bash
pip3 install pyspark
```
- Apache Kafka Streaming 
You can follow this [Click Here](https://linuxhint.com/install-apache-kafka-ubuntu-22-04/) to install.
- MYSQL Database : You can use ```Localhost``` database or ```remote sql``` database (recommended) using [https://www.db4free.net/](DB4FREE)

## INSTRUCTIONS
- Load the [database](https://github.com/smsraj2001/STREAM-BATCH-PROCESSING-KAFKA-SPARK/blob/main/Database/tweet_hashtags_db.sql) provided in the DATABASE FOLDER into MYSQL database and name the database.
- Ensure you have all these details of the dataset which you need to fill in kafka_producer.py
  - host="YOUR_HOST_NAME",
  - user="YOUR_USER_NAME_OF_DATABASE",
  - password="DATABASE_PASSWORD",
  - database="DATABASE_NAME"
- Open a terminal in the source folder of the project and run the following :
```bash
sudo systemctl start zookeeper
```
```bash
sudo systemctl start kafka
```
```bash
sudo systemctl status kafka
```
- If kafka is up and running successfully, let us create kafka topics (If kafka fails to start, please refer to installation guide again and fix the error) :
```bash
sudo -u "user_username" /opt/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic tweets
```
```bash
sudo -u "user_username" /opt/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic hashtags
```
```bash
sudo -u "user_username" /opt/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic top_tweets
```
```bash
sudo -u "user_username" /opt/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic top_hashtags
```
- After this we are ready to ```simulate the streaming``` of tweets and publish to the topics. Run the following command in a new terminal in the project folder :
```bash
python3 kafka_producer.py
```
- After successful running you will see the simulation of the tweets streaming which will be publishing to the respective topics.
- Open a second terminal in the project folder. Now we are ready to subscribe to the kafka topics.
- Now we will do subscribing by stream processing of data which is near to real time :
```bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.2.3 spark_streaming_consumer.py
```
- Now, open a third terminal in the project folder. We will do subscribing by batch processing of data :
```bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.2.3 spark_batch_consumer.py
```
