# Iceberg Learning
## Keywords: Netflix, Ryan Blue
## Where is Iceberg in the big data ecosystem
![image](https://github.com/user-attachments/assets/6df79855-37f8-4c08-8a3f-cce91cbb3742)
Figure source: [Starburst website](https://www.starburst.io/data-glossary/apache-iceberg/)  
Following Content Reference: [Apache Iceberg: An Architectural Look Under the Covers](https://www.dremio.com/resources/guides/apache-iceberg-an-architectural-look-under-the-covers/?utm_campaign=Search%20%2d%20Nonbrand%20%2d%20Iceberg%20%2d%20Global&utm_medium=cpc&utm_source=google&utm_term=iceberg%20architecture&campaignid=21254670688&adgroupid=161477881323&matchtype=e&gad_source=1&gbraid=0AAAAACVnO0rh7uKj8NeeNaRChvGadfdBW&gclid=Cj0KCQjwnui_BhDlARIsAEo9GuuZu3NTNYRmSexrdJZw7lQyMuf2S5Kh-3ReI5cocCApcftxqp-pfvUaAi1BEALw_wcB)
## Background: the de facto standard table format: Hive Table
### Basic idea
![image](https://github.com/user-attachments/assets/e1dca3d6-3a14-4073-8264-587d927a3152)
Directory-based - one directory per partition.
### Pros: Standard for 10 years,file-format agnostic (role as table format).
### Cons (data in the table is tracked at the folder level.):
- Difficult to change data. Files in FS can not be operated in a transactional manner. Replicating and switching the pointer is expensive. Also, concurrency is not safe.
- Cross-partition operations can not be done in one operation.
- Need to get the file list for directories, which takes a long time.
- User need to know how data is partitioned (physical layout). For example:
```
original filter:
WHERE event_ts >= ‘2021-05-10 12:00:00’
layout:
physical partitioning scheme (year=2021, then month=05, then day=10)
User then rewrite his query to:
WHERE event_ts >= ‘2021-05-10 12:00:00’ AND event_year >= ‘2021’ AND event_month >= ‘05’ AND (event_day >= ‘10’ OR event_month >= '06')
```
- The files in the same partition have the same prefix, so for cloud storage, they often go to the same node, which reduces the performance.

# File-level tracking Table Format Iceberg
![image](https://github.com/user-attachments/assets/6c462c6e-8267-4f81-9e05-d217de970dd3)
## Architecture
![image](https://github.com/user-attachments/assets/c22d1b76-da83-4fd9-949d-f44ec075b498)
### Metadata file
![image](https://github.com/user-attachments/assets/c8d4022c-4ebd-4bfe-8a98-caedb3b3de5e)
### Manifest list file and manifest files
![image](https://github.com/user-attachments/assets/0167a198-cf32-494c-9efd-a35f6c50410d)
Each manifest file contain a lot of useful information that is used to improve efficiency and performance while reading the data from these data files, such as details about partition membership, record count, and lower and upper bounds of columns.
![image](https://github.com/user-attachments/assets/82ac147e-8b1d-4aba-9e7c-54b94068fac8)
## How to use it: CRUD
### CREATE TABLE
```
CREATE TABLE table1 (
    order_id BIGINT,
    customer_id BIGINT,
    order_amount DECIMAL(10, 2),
    order_ts TIMESTAMP
)
USING iceberg
PARTITIONED BY ( HOUR(order_ts) );
```
![image](https://github.com/user-attachments/assets/1ae47933-7f61-4fd0-8852-049f422c3d00)
### INSERT
```
INSERT INTO table1 VALUES (
    123,
    456,
    36.17,
    '2021-01-26 08:10:23'
);
```
File changes happen at lower layers first, then upper layers.
![image](https://github.com/user-attachments/assets/43ae8214-53cc-43f8-a745-8a765cf9ec17)
### MERGE INTO / UPSERT
```
-- In this example, the stage table includes an update for the order that’s already in the table (order_id=123) and a new order that isn’t in the table yet, which occurred on January 27, 2021 at 10:21:46.
MERGE INTO table1
USING ( SELECT * FROM table1_stage ) s
    ON table1.order_id = s.order_id
WHEN MATCHED THEN
    UPDATE table1.order_amount = s.order_amount
WHEN NOT MATCHED THEN
    INSERT *
```
![image](https://github.com/user-attachments/assets/6b6f8f28-b84b-417f-86d1-805e72966fda)

When we execute this MERGE INTO statement, the following process happens:
When we execute this `MERGE INTO` statement, the following process happens:

1. The read path as detailed earlier is followed to determine all records in `table1` and `table1_stage` that have the same `order_id`.

2. The file containing the record with `order_id=123` from `table1` is read into the query engine’s memory (`00000-5-cae2d.parquet`), `order_id=123`'s record in this memory copy then has its `order_amount` field updated to reflect the new `order_amount` of the matching record in `table1_stage`. This modified copy of the original file is then written to a new Parquet file –  
   ```
   table1/data/order_ts_hour=2021-01-26-08/00000-1-aef71.parquet
   ```

   > 💡 Even if there were other records in the file that didn’t match the `order_id` update condition, the entire file would still be copied and the one matching record updated as it was copied, and the new file written out — a strategy known as copy-on-write. There is a new data change strategy coming soon in Iceberg known as merge-on-read which will behave differently under the covers, but still provides you the same update and delete functionality.

3. The record in `table1_stage` that didn’t match any records in `table1` gets written in the form of a new Parquet file, because it belongs to a different partition than the matching record –  
   ```
   table1/data/order_ts_hour=2021-01-27-10/00000-3-0fa3a.parquet
   ```

4. Then, a new manifest file pointing to these two data files is created (including the additional details and statistics) –  
   ```
   table1/metadata/0d9a-98fa-77.avro
   ```

   > 💡 In this case, the only record in the only data file in snapshot `s1` was changed, so there was no reuse of manifest files or data files. Normally this is not the case, and manifest files and data files are reused across snapshots.

5. Then, a new manifest list pointing to this manifest file is created (including the additional details and statistics) –  
   ```
   table1/metadata/snap-9fa1-3-16c3.avro
   ```

6. Then, a new metadata file is created based on the previously current metadata file with a new snapshot `s2` as well as keeping track of the previous snapshots `s0` and `s1`, pointing to this manifest list (including the additional details and statistics) –  
   ```
   table1/metadata/v3.metadata.json
   ```

7. Then, the value of the current metadata pointer for `db1.table1` is atomically updated in the catalog to now point to this new metadata file.
### SELECT *
![image](https://github.com/user-attachments/assets/68671ed0-5961-49cd-922f-28d65e3b9a64)
### SELECT with hidden partition
```
SELECT *
FROM table1
WHERE order_ts = DATE '2021-01-26'
```
![image](https://github.com/user-attachments/assets/29d38712-1211-4f5a-904f-f16432e89ee2)
step 3. It then opens this metadata file, retrieves the entry for the manifest list location for the current snapshot s2. It also looks up the partition specification in the file and sees that the table is partitioned at the hour level of the order_ts field.

### Timetravel: rely on snapshots, ruled by GC policy

## Design Benefits
- Snapshot isolation for transactions. Read and write won't interfere each other. Now write is with OCC, and atomic.
- Faster planning and execution. No need to do some FS operation at runtime. like ls. Statistics in manifest files are updated on write-path, never get stale.
- Abstract the physical, expose a logical view. 
