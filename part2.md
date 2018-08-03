# Part II - Starting an ELK on LinuxONE for Monitoring Cryptographic Activities

In this part, you will learn how to monitor captured LinuxONE crypto activity thanks to APIs. You will send these captured information to a no-slq database (Elasticsearch Database).

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

## 1. Seting-up an ELK infrastructure 
An ELK stack can be implemented very easily, not matter the processor architecture.

If you want to monitor a LinuxONE Crypto activities with an ELK running on x86, please follow the following instructions:
- Deploying a docker ELK on Linux: https://github.com/deviantony/docker-elk

If you want to monitor a LinuxONE Crypto activities with an ELK running also on LinuxONE, please follow the following instructions:
- Building Elasticsearch on LinuxONE : https://github.com/linux-on-ibm-z/docs/wiki/Building-Elasticsearch
- Buidling Logstash on LinuxONE : https://github.com/linux-on-ibm-z/docs/wiki/Building-Logstash
- Building Kibana on LinuxONE : https://github.com/linux-on-ibm-z/docs/wiki/Building-Kibana

(Recommanded) If you want to monitor a LinuxONE Crypto activities after deploying a docker ELK on Linux:LinuxONE, please follow the following instructions:

Required tool:
```
sudo apt-get install git docker docker-compose
```

Required dockerfile:
```
sudo git clone https://github.com/guikarai/ELK-CPACF.git
```

Building a kibana docker image for s390 architecture:
```
sudo docker build -t "kibana:Dockerfile" .
```

Building an elasticsearch docker image for s390 architecture:
```
sudo docker build -t "elasticsearch:Dockerfile" .
```

Starting up ELK:
```
sudo docker-compose up -d
```

Please verify that the ELK Stack is properly started issuing the following command:
```
root@crypt06:~# sudo docker ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                                            NAMES
fc2242672599        dockerelk_kibana          "/bin/bash /usr/lo..."   22 hours ago        Up 22 hours         0.0.0.0:5601->5601/tcp                           dockerelk_kibana_1
8f87424acd61        dockerelk_elasticsearch   "/usr/local/bin/do..."   22 hours ago        Up 22 hours         0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   dockerelk_elasticsearch_1
```

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
