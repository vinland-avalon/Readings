# Debezium
Debezium is a distributed platform that converts information from your existing databases (such as MySQL) into event streams, enabling applications (such as Elasticsearch) to detect, and immediately respond to row-level changes in the databases.
## Reference
- [debezium.io](https://debezium.io/documentation/reference/2.3/tutorial.html)
## How it works
Debezium is built on top of *Apache Kafka* and provides a set of Kafka Connect compatible connectors. In fact, Debezium is specifically designed as a set of connectors that integrate with Kafka. Each of the connectors works with a specific database management system (DBMS). Connectors record the history of data changes (binlog, if for MySQL) in the DBMS by detecting changes as they occur, and streaming a record of each change event to a Kafka topic. Consuming applications can then read the resulting event records from the Kafka topic.
### How to handle schema change
Debezium is designed to handle such changes dynamically and adapt to the new schema automatically. However, Elasticsearch may not handle some schema changes automatically, especially if they involve drastic changes in the data structure or if the changes violate certain constraints. In such cases, you may need to manually adjust the Elasticsearch mapping to accommodate the schema changes appropriately.
## How to use it?
The bill platform of ByteDance:  
MySQL -> Debezium -> Kafka -> Kafka Connect Elasticsearch Sink Connector -> ElasticSearch  
ps: Kafka Connect is a framework for building and running connectors to move data between Kafka and other data systems. The Elasticsearch Sink Connector is a pre-built connector that allows you to consume data from Kafka topics and index it into Elasticsearch.