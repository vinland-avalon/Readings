# Architecture (divide-and-conquer design)
![fdb](https://github.com/vinland-avalon/Readings/blob/main/images/FDB_Architecture.png?raw=true)
## Bootstrapping
Coordinators elect single ClusterController (with disk Paxos),  
newly elected ClusterController recruits a new Sequencer,  
newly Sequencer 1) read configs about old LS from Coordinators, 2) spawns new TS and LS,  
Proxies recover system data, including info about all SS,  
newly Sequencer waits until the new TS finishes recovery, and then write new LS's info to all Coordinators,  
TS begin to process incoming transactions.
## Reconfiguration
When some servers fail or configs get changed:
Sequencer will monitor Proxies, Resolvers and LS, if they fail or get reconfigured, Sequencer process terminates and get detected by ClusterController, who will recruit new Sequencer and start bootstrapping again.
## Transaction
The processing is shown as figure. Notice, writes are buffered locally at Client first.
### Commit and OCC (read-write transactions)
The commit (or abort) is divided into 3 steps. First, contact Sequencer to get commit version, which is advanced at rate of 1 million per second. Second, transaction info is sent to range-partitioned Resolvers to check read/write conflict with OCC. Finally, if commit is allowed, LS to persistent. (SS will do in background). Besides this read-write transactions shown in figure, FDB also supports
- read-only transactions
- snapshot reads 
### Support Strict Serializability
FDB implements Serializable Snapshot Isolation (SSI) (PG also implements this) by combining OCC with MVCC. The version assigned by Sequencer is actually the Log Sequence Number (LSN), which is exactly the order to process.  
The lock-free conflict detection algo of Resolver is as follows:
![fdb_conflict_algo](https://github.com/vinland-avalon/Readings/blob/main/images/FDB_Conflict_Detect.png?raw=true)
### Logging Protocol
![fdb_logging](https://github.com/vinland-avalon/Readings/blob/main/images/FDB_Logging.png?raw=true)
The SS will aggressively fetch redo logs before they get durable on LS. Normally within 4ms thus guarantee the high performance.
### Transaction Recovery
In FDB, the recovery is purposely made very cheap — there is no checkpoint, and no need to re-apply redo or undo log.

# Simulation Test Framework
# 5s MVCC Window
From our experience, this 5s window is long enough for the majority of OLTP use cases. If a transaction exceeds the time limit, it is often the case that the client application is doing something inefficient, e.g., issuing reads one by one instead of parallel reads.

