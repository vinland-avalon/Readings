
# What problem is the paper trying to solve?

For collecting and delivering **high volumes** of log data with **low latency**.

# Why is the problem important?

The “log” data typically includes (1) user activity events; (2) operational metrics. Log data has long been a component of analytics used to track user engagement, system utilization, and other metrics.  
However recent trends in internet applications have made activity data a part of the production data pipeline used directly in site features. **This production, real-time usage of log data creates new challenges for data systems because its volume is orders of magnitude larger than the “real” data.**  
Also, at LinkedIn, the team found that in addition to traditional offline analytics, we needed to support most of the real-time applications mentioned above with delays of no more than a few seconds.

# What sets it apart from prior work?

Combines the benefits of traditional log aggregators and messaging systems, which means "high throughput" and "nearly real time" at the same time and mitigate the drawbacks.

The drawbacks of those two kinds of systems can be summarized as below.

Message Systems:
- Traditional enterprise messaging systems mismatch in features, focusing on offering a "too-rich" set of delivery guarantees. Those unneeded features tend to increase the complexity of both the API and the underlying implementation.
- Also, many systems do not focus as strongly on throughput as their primary design constraint.
- Third, those systems are weak in distributed support.
- Finally, many messaging systems assume near immediate consumption of messages.

Log Aggregators:
- Most of those systems are built for consuming the log data offline, and often expose implementation details unnecessarily.
- Push model often does not work.


# What are the key technical ideas?

Kafka employs a pull-based consumption model that allows an application to consume data at its own rate and rewind the consumption whenever needed. By focusing on log processing applications, Kafka achieves much higher throughput than conventional messaging systems. It also provides integrated distributed support and can scale out.

## The main workflow:
A stream of messages of a particular type is defined by a *topic*. A producer can publish messages to a topic. The published messages are then stored at a set of servers called
*brokers*. A consumer can subscribe to one or more topics from the brokers, and consume the subscribed messages by pulling data from the brokers.
![Kafka_architecture](https://github.com/vinland-avalon/Readings/blob/main/images/Kafka_architecture.png?raw=true)
## Simple API
## Efficiency on a Single Partition
- Simple storage: No message id but be addressed with offset. Consumer *pull* with offsets.
- Efficient transfer: Batch process. Only FS-layer cache to avoid double cache overhead. Since the writes and reads are sequetial, operating system caching heuristics are very effective (specifically write-through caching and readahead).
- Stateless broker: The offset is maintained by customer itself. However, it is hard to delete messages for broker then. And Kafka will just delete if it has been retained for a specific period.
## Distributed Coordination (focus on consuming)
- Make a partition within a topic the smallest unit of parallelism.
- Let consumers coordinate among themselves in a decentralized fashion.
- Introduce Zookeeper.
## Delivery Guarantees
At-least-once delivery, instead of Exactly-once delivery.
To avoid log corruption, Kafka stores a CRC for each message in the log.

# What are the main areas of improvements and open questions?

- Add built-in replication of messages across multiple brokers to allow durability and data availability guarantees.
- Add some stream processing capability.
