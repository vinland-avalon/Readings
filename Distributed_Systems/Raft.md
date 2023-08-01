# Raft
Almost the most understandable distributed consensus.
## Condensed Summary of Raft
![condensed_summary_of_raft](https://github.com/vinland-avalon/Readings/blob/main/images/condensed_summary_of_raft.png?raw=true)
## When to commit
For raft, both read and write operations are sent to leader (from clients).  
The leader will then replicate such log to followers. If the log are accepted enough (majority og the cluster), then the log will lead to increment of `commitIndex` of leader. In the following AppendEntries RPC, the log will be applied, since the commitIndex has been changed.
## My implementation is [here](https://github.com/vinland-avalon/Distributed-Systems)