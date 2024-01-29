
# What problem is the paper trying to solve?

In summary, about how to manage and process large-scale data sets in a efficient, reliable and cos-effective way.  
This is a paper not solving a specific problem. The team observed modern file systems and found some limitations and then provided a comprehensive project to solve them. Such limitations are as follows.
- Component failures are more normal, leading to issues related to constant monitoring, error detection, fault tolerance, and automatic recovery
- File size is extremely increasing, leading to issues related to I/O operation and blocksize design.
- Most modification are *appending* (also the reading is also sequential), indicating potential optimization and questiosn about caching.
- There could be better APIs for FS. For example, atomic operations and aggregation functions.


# Why is the problem important?

A robust and scalable file system is essential for supporting Google's various applications, which are always large-scale web services, big data analytics, and cloud computing applications, which form the foundation of modern internet infrastructure.  
When it comes to detials, we can view considerations related to this question one by one.
- Fault Tolerance. The inexpensive components are maybe out of control at any given time and some will not recover from their current failures. It is crucial to have a file system that can detect, tolerate, and recover promptly from component failures on a routine basis.
- Increasing data. Multi-GB files are common now and the number of files to be managed could also reach billions. If not solved, operations will get slower and more expennsive.
- Optimization on "appending" operations. This is a good attibutes discovered with observation. If optimized according to it, performance will get improved, in both throughout and low latency.
- Redesign the file system API. It benefits the overall system by increasing the team's flexibility.

# What sets it apart from prior work?

- Like AFS, GFS's namespace is independent from location. However, GFS also spreads the data across servers for aggregate performance and increased fault tolerance.
- GFS does not provide any cache semantics.
- Unlike like Frangipani, xFS, Minnesota’s GFS[11] and GPFS [10], opts for the centralized approach in order to gain flexibility, implement features and increase its reliability.
- Use write-ahead-log to made updates persistent.
- Compared to Lustre, provides aggregation functions but only focus on the needs.
- Unlike the NASD work, its chunkservers use lazily allocated fixed-size chunks rather than variable-length objects. Also, it implements many reliability guarantees.
- About how to solve "atomic updates", different from River, it uses a persistent file, which is good for fault tolerance but limited to n-to-1 queues efficiency.
- GFS relaxes its consistency model to simplify the file system without imposing an onerous burden on applications.

# What are the key technical ideas?

## Different Interface
Including *create, delete, open, close, read, write, snapshot and record append*.
## Architecture. 
- Centralized single *master* plus multiple *chunkservers*. Files are divided into fixed-size chunks, which is replicated on multiple chunkservers. Master stores metadata, manage *lease* and control lots of activities. When communicating, clients interact with master for metadata then with chunkservers for data. As the following figure shows.
![gfs-architecture](https://github.com/vinland-avalon/Readings/blob/main/images/gfs-architecture.png?raw=true)
- A bigger chunk size, which reduce workloads of master and avoid internal storage waste. It's a trade-off for smaller files.
- Operation Log. The operation log contains a historical record of critical metadata changes within master. It is important so both written on a local machine and a remote machine. There are also checkpoints to compact logs.
- Consistency Model. GFS provides atomic guarantees for file namespace mutations while is more relaxed for data mutations. 
## System Interactions
- Mutation order. It is guranteed with *lease*. Master decides a *primary chunkserver* and gives the lease to it. The the primary one decides order of mutations and asks to apply to all replicas, thus minimize the overhead of master.
- The data flow and the control flow are seperated.
- Atomic Record Appends. Similar to data flow, but with examinination about if the size exceed.
## Master Operations
- Manage namespace with read-write locks.
- Chunkreplicas are created for three reasons: chunkcreation, re-replication, and rebalancing.
- Grabage Collection. After a file is deleted, GFS does not immediately reclaim the available physical storage. It does so only lazily during regular garbage collection at both the file and chunklevels.
## Fault Tolerance
- Chunkserver replicas and master shadows.
- Each chunkserver uses checksumming to detect corruption of stored data.

# What are the main areas of improvements and open questions?
## Main Areas of Improvements
The performance measurements provide insights into the scalability and throughput of GFS, showcasing the system's capabilities in handling large-scale workloads. It also discusses the minimal performance impact of logging, highlighting that the benefits of logging outweigh the associated costs. The measurement also compares the performances of "mutation" and "append log" and shows how the latter one is oprimized better.
## Open Questions
This paper does not point out future works in a section. However, we can discover some across the whole paper. First, the GFS is based on observation of Google services, so there are some assumptions may be wrong in other scenarios. Second, there is only one master. Although its workload is reduced, it may lead to future problems.


