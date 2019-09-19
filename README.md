# easyELK

easyELK is a script that will install ELK stack 7.x on your system.

Great for users who want to install quickly or for those who are new to ELK and want to get up and running with less confusion.  Searching on the web and even using the installation guides on [elastic's website](https://www.elastic.co) can be confusing.  easyELK will cut out most of the installation frustration.

## Prerequisites

- System must be Debian-based / Ubuntu
- Other OS flavors like RedHat , Centos, OpenSuSE require rpms and those are not supported with this script
- Recommend you have a static IP set for your system.
- Must be able to elevate to root

## How To Use

1. Copy easyELK to the system where you want to run the ELK stack
2. Make easyELK executable 
```sudo chmod +x easyELK```
3. Run easyELK
```sudo ./easyELK```

easyELK will perform the following steps:

1. System Update
2. Install Java 8 as required to run ELK
3. Install Elasticsearch 7.x
4. Install Kibana 7.x
5. Install Logstash 7.x
6. Set Elasticsearch, Kibana, and Logstash to start on boot
7. Start all three services service

## Post Installation
After the installation completes, follow the configuration guides below:

[Elasticsearch] (https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html)

[Kibana] (https://www.elastic.co/guide/en/kibana/current/settings.html)

[Logstash] (https://www.elastic.co/guide/en/logstash/current/configuration.html)

Check out [Beats] (https://www.elastic.co/products/beats) and prepare data shippers.

