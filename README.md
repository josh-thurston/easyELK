# easyELK

easyELK is a script that will install ELK stack 7.x on your system.

Great for users who want to install quickly or for those who are new to ELK and want to get up and running with less confusion.  Searching on the web and even using the installation guides on [elastic's website](https://www.elastic.co) can be confusing.  easyELK will cut out most of the installation frustration.

## Prerequisites

- System must be Debian-based / Ubuntu.  Not ARM architecture
- Other OS flavors like RedHat, Centos, OpenSuSE require rpms and those are not supported with this script
- Recommend you have a static IP set for your system.
- Must be able to elevate to root

## How To Use

1. Copy easyELK to the system where you want to run the ELK stack
2. Make easyELK executable 

```
sudo chmod +x easyELK
```

3. Run easyELK

```
sudo ./easyELK
```

easyELK will perform the following steps:

1. System Update
2. Install Java 8 as required to run ELK
3. Install Elasticsearch 7.x
4. Install Kibana 7.x
5. Install Logstash 7.x
6. Set Elasticsearch, Kibana, and Logstash to start on boot
7. Start all three services service

# Post Installation

## Configure Elasticsearch
 
 Using vim or nano, open elasticsearch.yml
 
 ```
 sudo nano /etc/elasticsearch/elasticsearch.yml
 ```
 
 Scroll down to the Network Section
 
 1. Network Section - Uncomment network.host: and http.port:
 2. Change the IP Address to 127.0.0.1 or 0.0.0.0

```
network.host: 0.0.0.0
http.port: 9200
```

Note:

    - 127.0.0.1 will set Elasticsearch to accept connections from the localhost only
    - 0.0.0.0 will set Elasticsearch to accept connections from any host (i.e. remote systems)
    - If you set network.host to 0.0.0.0 you must set the seed hosts
 3. Discovery Section - add the following if network host is 0.0.0.0
 
```
discovery.seed_hosts:
   - 0.0.0.0:9300
   - IP_ADDRESS:9300 # Put your elasticsearch host IP
```

 4. Exit --> Save
 5. Restart the Elasticsearch service and check that it is up and running

 ```
 sudo systemctl restart elasticsearch.service
 ```
 
 ```
 service elasticsearch status
 ```

```
curl -X GET http://localhost:9200
```

This should return a JSON output similar to this:.

```
{
  "name" : "elk",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "kM8aJLuAQfKXjTO4VJLiKw",
  "version" : {
    "number" : "7.3.2",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "1c1faf1",
    "build_date" : "2019-09-06T14:40:30.409026Z",
    "build_snapshot" : false,
    "lucene_version" : "8.1.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## Configure Kibana
 
 ```
 sudo nano /etc/kibana/kibana.yml
 ```
 
 1. Uncomment server.host: 
 
    - Choose "localhost" to allow connections to Kibana from the local system only
    - Change to change to 0.0.0.0 to allow connections from any host (i.e. remote systems)
 
 2. Uncomment elasticsearch.host: ```"http://localhost:9200"```
 3. Exit --> Save
 5. Restart the Kibana service and check that it is up and running
 
 ```
 sudo systemctl restart kibana.service
 ```
 
 ```
 service elasticsearch status
 ```

The Kibana web page will be ready after a couple minutes. You can connect by visiting the server url.  *Note:* If a message saying the server is not read, wait a few minutes and refresh.
 
 ```
 http://Kibana IP:5601
 ```
  
## Configure Logstash

Logstash configuration is not as straight forward as Elasticsearch and Kibana.  Typically, it is best to refer to the [Elastic config guide](https://www.elastic.co/guide/en/logstash/current/configuration.html)

A couple key pointers:

1. Understand the Structure is good, but often times you will find config info for most of the datasources you want to setup
2. Understand the Event Processing Pipeline (inputs --> filters --> outputs).  Where is the data coming from, how will it be filtered, and where is the data shipped?
3. You can, and most likely will, have multiple pipelines.  Learn how to work with pipelines.yml and logstash.conf

## Configure Beats

The script installed Filebeat, Auditbeat, Packetbeat, and Metricbeat on the server.  Check configure each of them to your specifications.

## Beats

The goal is to get data, and the last piece is data collection.  Beats provides multiple 'shippers' to assist in data collection. For information on configuring Beats Family modules visit [Elastic Beats](https://www.elastic.co/products/beats)

Use my [easyBEATS](https://github.com/josh-thurston/easyBEATS) solution for Debian (Ubuntu), Mac, and Raspberry Pi (ARM) setup.

## Additional Reference Materials
Fore more detailed information on ELK, visit the Elastic configuration guides below:

[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html)

[Kibana](https://www.elastic.co/guide/en/kibana/current/settings.html)

[Logstash](https://www.elastic.co/guide/en/logstash/current/configuration.html)


