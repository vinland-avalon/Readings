# Basic Info
Title: [ALEX: An Updatable Adaptive Learned Index](https://dl.acm.org/doi/10.1145/3318464.3389711)
# First Impression
It take the advantage of both `B+ tree` and [`Learned Index model of Dr. Tim Kraska`](https://dl.acm.org/doi/10.1145/3183713.3196909)[1] and try to make the index tolerant of operations including `insert`.
## Inspiration
- Gapped Array and how to handle insert
- "Piecewise" linear regression model -> linear regression model in different nodes.
- About retrain. In some cases, no need to retrain, but scale.
- Whether to retrain can be judged by deviation between *empirical cost* and *expected cost*. That is because, even the prediction is not that accurate, can also reach data with search. Re-train is just an optimization.
- The cost model is important. Almost every operation is under the guidance of it.
# How does it work
It is more like B+ tree, compared to `RMI` model in [1] to support updates.
- Data Layout. The leaf nodes are seperated, so the data wont be shifted too much when inserting.
- Data Layout. The leaf nodes is in format of `Gapped Array`, which means, there will be free space for incoming inserted data. Also, it is managed with some schema.
- Expotional Search. It is not about updating, but about querying/lookups. It is better than [1]'s binary search, since the wanted data may be around predicted position.
- Model-Based Insertion. When inserting, first predict, then insert at exactly that position. However, Maybe this is not suitable for LSMT.
- A cost-model. And no need to re-tune model for different datasets.
# Node Layout
## Data Node (leaf node)
Each Data Node contains a linear regression model, (not "P"LR), two Gapped Arrays for keys and payloads. The single key and the single payload is fix-sized, the latter one could be pointer.  
There are bitmaps to skip gaps.
## Internal Node
Each Internal Node contains a linear regression model, as well as an array of pointers pointing to child nodes.  
Unlike [1], which is to seperate next layer equally, the main purpose of Internal Node in ALEX is to gather data that is almost linear in one child node and seperate data with different distribution (non-linear) in different child nodes.
![internal_nodes](https://github.com/vinland-avalon/Readings/blob/main/images/ALEX_Internal_Nodes.png?raw=true)
# Algorithms (lookups, range query, insert, delete, out of bounds insert, bulk load)
## Lookups and Range Queries
## Insert in non-full nodes
First, just like lookups, to get the position. Then if the position if at a gap, then insert. Else, shift the other elems by one postition in the direction of closest gap, then insert.
## Insert in full nodes
simple cost model -> expansion / various of split
### Expansion
Define lower and upper density limits as $d_l$ and $d_u$, which is usually 0.6 and 0.8. The if the number of keys reaches $d_u$, then expand the slots to $n/d_l$. The Linear Regression need to be scaled or retrained.
### Split
Can simply regard it like B+ tree.
### Insert Algorithm
If the empirical cost has deviated from the expected cost (It is defined as 50%, which means the model is not accurate anymore), we must either (i) expand the data node and retrain the model, (ii) split the data node sideways, or (iii) split the data node downwards. Or can just expand data node and **scale** it.
#### About Why empirical cost deviated from expected cost.
This means the model is inaccurate, after a period after creation. Normally caused by abnormal data distribution.
## Insert out of bounds
It is a special kind of insert where the new data is out of key space. The handling is actually a trick for special case, such as a series of append-only inserts. If it is detected so by data node, then algorithm tend to expand first and insert there.
## Bulk Load
It is used when building or rebuilding index with lots of data. The target is to build a better tree.  
When building downwards, for each node, first make a decision about whether it should be an internal node of a data node. Then, if former, how about its fanout. The decisions are made according to a proposed **Fanout Tree**  


