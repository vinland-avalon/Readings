# HTAP Databases: What is New and What is Next
## Info
SIGMOD ’22  
Tsinghua University: Guoliang Li, Chao Zhang
## HTAP Databases
![htap-databases](https://github.com/vinland-avalon/Readings/blob/main/images/htap-databases.png?raw=true)  
Notice, from my understanding, this kind of taxonomy is not exclusive. It's more like several classic cases.
### a) Primary Row Store+In-Memory Column Store
#### Examples
- Oracle Database In-Memory: A Dual Format In-Memory Database
- Real-Time Analytical Processing with SQL Server
- MariaDB. Deploy an HTAP Server with MariaDB ColumnStore 5.5 and Community Server 10.6, 2021.
- PolarDB. PolarDB HTAP Real-Time Data Analysis Technology Decryption
- DB2 with BLU Acceleration: So Much More Than Just A Column Store.
#### My question
https://chatgpt.com/share/66f038a6-e4cc-800c-9681-11d68dbeeb46
1. Does it really make sense to have in-memory column store for analytical processing? I mean, given analytical processing often involves "large scan", which I assume cannot be stored in memory. Then there will be a lot of memory replacement and such in-memory "buffer" should not work.  
ChatGPT said: Working Set Size
2. Then Do you have statistics about the comparison about in-memory row store and column store?  
ChatGPT said: Column cache still outperform because it can scan less data, compress better and with better IO efficiency and scalability. And some product have been up to 10x faster than traditional row-based databases.
### (b) Distributed Row Store+Column Store Replica
#### Examples
1. TiDB: A Raft-based HTAP Database
#### Workflow
Rely on distributed architecture. Some slave nodes are in format of column store, in asynchronous way. The benefit is that the separation is good - the workload isolation level is high as transactions are processed on nodes with a row store, and analytical queries are executed on nodes with a column store. The shortcoming is freshness.
### (c) Disk Row Store+Distributed Column Store
#### Examples
1. MySQL Heatwave. Real-time Analytics for MySQL Database Service, 2021.
#### Workflow
Utilizes a disk-based RDBMS with a distributed in-memory column-store (IMCS). Notice, the "disk row store" and "distributed row store" in (c) and (b) are different. The former one is more like traditional RDBMS such as MySQL, which has limited distribution (sharding or replication) while the latter one is more newly-proposed, with distribution and maybe mixed with in-memory and disk.
### (d) Primary Column Store+Delta Row Store
#### Examples
- The SAP HANA Database–An Architecture Overview. 
- Efficient Transaction Processing in SAP HANA Database: The End of A Column Store Myth.
#### SAP HANA
It divides the in-memory data store into three layers: L1-delta, L2-delta, and Main. The L1-delta keeps data updates in a row-wise format. When the threshold is reached, the data in L1- delta is appended to L2-delta. The L2-delta transforms the data into columnar data, then merges the data into the main column store. Finally, the columnar data is persisted to the disk storage.


