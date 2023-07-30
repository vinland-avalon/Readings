# Basic Info
[Apache hive](https://hive.apache.org/)
# First Impression
Although it is called data warehouse, we'd better regard it as a query engine for Hadoop, as an abstraction of MapReduce and based on HDFS.
## Features
- SQL-like. Instead of writing Java code for MapReduce. Deal with structured data.
- Abstraction of MapReduce/Spark.
- Store with HDFS or Amazon S3.
## Limitations
- Maybe bring performance overhead.
- Latency. Not for OLTP.
- Does not support updates or deletes.
## How does it work
![hive_architecture](https://github.com/vinland-avalon/Readings/blob/main/images/hive_architecture.png?raw=true)
- Hive Metastore(HMS). Store Metadata of tables in RDBMS (installed MySQL/PostgreSQL or internally embedded derby), instead of HDFS. It tells us to be flexible. RDB is faster for smaller datasets. Also, in real time, we use remote RDBMS instead of embedded derby. Easy to understand, local metadata is hard to use in distributed cases.
## reference
[youtube](https://www.youtube.com/watch?v=taTfW2kXSoE)
[simplilearn](https://www.simplilearn.com/what-is-hive-article)

