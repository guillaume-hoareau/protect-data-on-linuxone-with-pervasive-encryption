# Welcome in Step 2 about building and deploying ELK docker images to IBM Cloud private

In this part, you will learn how to monitor captured LinuxONE crypto activity thanks to APIs. You will send these captured information to a no-slq database (Elasticsearch Database).

## Agenda of this Step 2 is the following:
1. What the ELK..?!
2. What to Keep in mind about ELK?
Build the Docker image
2. Deploy the docker image to IBM Cloud private

# ELK Stands for ElasticSearch Logstach Kibana

## What the ELK..?!
Elasticsearch is a search engine based on Lucene. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and is released as open source under the terms of the Apache License. Official clients are available in Java, .NET (C#), PHP, Python, Apache Groovy, Ruby and many other languages. According to the DB-Engines ranking, Elasticsearch is the most popular enterprise search engine followed by Apache Solr, also based on Lucene.

Elasticsearch is developed alongside a data-collection and log-parsing engine called Logstash, and an analytics and visualisation platform called Kibana. The three products are designed for use as an integrated solution, referred to as the "Elastic Stack" (formerly the "ELK stack").

Elasticsearch can be used to search all kinds of documents. It provides scalable search, has near real-time search, and supports multitenancy. "Elasticsearch is distributed, which means that indices can be divided into shards and each shard can have zero or more replicas. Each node hosts one or more shards, and acts as a coordinator to delegate operations to the correct shard(s). Rebalancing and routing are done automatically". Related data is often stored in the same index, which consists of one or more primary shards, and zero or more replica shards. Once an index has been created, the number of primary shards cannot be changed.

More information about ELK here: https://www.elastic.co

## What to Keep in mind about ELK?
"ELK" is the acronym for three open source projects: Elasticsearch, Logstash, and Kibana. 

* Elasticsearch is a search and analytics engine. 
* Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. 
* Kibana lets users visualize data with charts and graphs in Elasticsearch. 

The Elastic Stack is the next evolution of ELK.

# Build the Docker image 
The objective is to build a Docker image from the banking application and then deploy it to the IBM Cloud private.

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build, users can create an automated build that executes several command-line instructions, step by step.


## 2. Seting-up crypto data collection
Please, correct the default ESserverIP adress with your @IP adress according to your environment.
Let's start with the script in charge to collect data from the icastats command:
```
sudo vi icastats.sh
#!/bin/bash
ESserverIP="18.197.196.0" <--- Change with your IP address here
```

So see with a user friendly interface the status of your elasticsearch instance, please, install in your computer the elasticsearch web-plugin named elasticsearch-head. Fill in the form and connect to your elasticsearch instance with the appropriate IP adress. The portname is by default 9200. You should be able to see something simillar to the following:
![alt text](https://github.com/guikarai/ELK-CPACF/blob/master/images/Capture%20d%E2%80%99e%CC%81cran%202018-06-20%20a%CC%80%20170639%20(2).png)

It is now time to feed your elastic search with collected data and to create an index on elasticsearch database. Please issue the following command:
```
sudo chmod +x icastats.sh
sudo ./icastats.sh
{"_index":"monitor-icastats","_type":"icastats","_id":"RD8FkmMBF84PFKnZKoVW","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}
{"_index":"monitor-icastats","_type":"icastats","_id":"RD8FkmMBF84PFKnZKoVW","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}
[...truncated...]
```

Ervery 5 seconds, a record will be sent to the elasticsearch db. 

To assess that it works properly, with web interface there are new records added in the elasticsearch db.
Your elasticsearh web interface should look like the foolowing:
![alt text](https://github.com/guikarai/ELK-CPACF/blob/master/images/Capture%20d%E2%80%99%C3%A9cran%202018-05-24%20%C3%A0%20140351%20(2).png)

You are now good for the part III about creating a dashboard to magnify live captured crypto information.
