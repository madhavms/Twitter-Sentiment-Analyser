# Sentiment Analysis on Real-Time Twitter Data.

A real time streaming ETL pipeline for Twitter data is implemented using Apache Kafka, Apache Spark and Delta Lake Database. Sentiment analysis is performed using Spark Machine Learning libraries on the streaming data, before being written to the database.

## Architecture:

![Image](https://github.com/madhavms/Twitter-Sentiment-Analyser/blob/main/Images/Architecture.jpg?raw=true)

## Requirements:

* kafka==1.3.5
* pyspark==3.2.1
* tweepy==3.10.0

## Usage:

### Setting up Kafka Environment:

STEP 1: GET KAFKA
Download the latest Kafka release and extract it:

```
$ tar -xzf kafka_2.13-3.1.0.tgz

$ cd kafka_2.13-3.1.0 
```

STEP 2: START THE KAFKA ENVIRONMENT
NOTE: Your local environment must have Java 8+ installed.

Run the following commands in order to start all services in the correct order:

### Start the ZooKeeper service
### Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
```
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

Open another terminal session and run:

### Start the Kafka broker service:
```
$ bin/kafka-server-start.sh config/server.properties
```
Once all services have successfully launched, you will have a basic Kafka environment running and ready to use.

STEP 3: CREATE A TOPIC TO STORE YOUR EVENTS
Kafka is a distributed event streaming platform that lets you read, write, store, and process events (also called records or messages in the documentation) across many machines.

Example events are payment transactions, geolocation updates from mobile phones, shipping orders, sensor measurements from IoT devices or medical equipment, and much more. These events are organized and stored in topics. Very simplified, a topic is similar to a folder in a filesystem, and the events are the files in that folder.

So before you can write your first events, you must create a topic. Open another terminal session and run:
```
$ bin/kafka-topics.sh --create --topic twitterdata --bootstrap-server localhost:9092
```


### Installing the python dependencies:
```
pip install -r requirements.txt
```

### Running the python programs:

Once all the Kakfa servers are running the python applications can be executed in the below order:

#### Run the Kafka Producer to write to the Kafka topic:
```
python producer.py
```
#### Run the Kafka Consumer to consume data from Kafka and perform ML analysis using SparkML
```
python consumer.py
```

#### Run the program to read processed sentimental data from the Delta Lake and write to sink for further visualisation.
```
python read_delta_stream.py
```

## ML Pipeline for Sentiment Analysis

Stanford's [Sentiment140]https://www.kaggle.com/kazanova/sentiment140 dataset is used to train an ML pipeline consisting of five stages. Spark ML libraries were used for creating the ML pipeline. 

![Image](https://github.com/madhavms/Twitter-Sentiment-Analyser/blob/main/Images/ML%20Pipeline.jpg)

Below are the 5 stages of estimation:

StopWordRemover: It takes input as a sequence of strings and drops all the stop words from the input data.

![Image](https://github.com/madhavms/Twitter-Sentiment-Analyser/blob/main/Images/StopWordRemover.png)


CounterVectorizer: It represents the words in the text in the form of numerical data instead of a sentence. This is easier for the machine to understand and for machine learning is made simpler.
	
text = [???Hello my name is james, this is my python notebook???]

![Image](https://github.com/madhavms/Twitter-Sentiment-Analyser/blob/main/Images/CounterVectorizer.png)


Inverse Document Frequency: It is a measure of how important a word is in a collection of  documents.

Label Indexing: It is done on the sentiment value column present in the Sentiment140 dataset. The column labels are converted to indices of [0, number of labels]. Since there are only two sentiment values in the dataset 4 - positive and 0 - negative which will be labelled as 0 and 1. 0 will be applied to the most frequent label and it will be labelled in descending order.

Logistic Regression: It is a very well known algorithm used to predict categorical outcome. We use binary logistic regression here to predict a binary outcome that has two categorical outcomes namely positive or negative.






