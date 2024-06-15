# Consistency, Availability and Partition Tolerence

- When there's no network partition, which is impossible, distributed system can always do both consistency and availability
- When there's network partition, the design must choose one, whether instances to proccess requests or to keep consistency.
- I guess CP is more common, since we always want the machine to answer right. Like Raft.
- Here, for Consistency, we refer to it as Strong Consistency, rather than Weak or Eventual Consistency.

