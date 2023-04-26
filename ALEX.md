# Basic Info
Title: [ALEX: An Updatable Adaptive Learned Index]()
# First Impression
## It take the advantage of both `B+ tree` and [`Learned Index mode of Dr. Tim`]()[1] and try to make the index tolerant of operations including `update`.
# How does it work
It is more like B+ tree, compared to `RMI` model in [1] to support updates.
- Data Layout. The leaf nodes are seperated, so the data wont be shifted too much when inserting.
- Data Layout. The leaf nodes is in format of `Gapped Array`, which means, there will be free space for incoming inserted data. Also, it is managed with some schema.
- Expotional Search. It is not about updating, but about querying/lookups. It is better than [1]'s binary search, since the wanted data may be around predicted position.
- Mode-Based Insertion. When inserting, first predict, then insert at exactly that position. However, Maybe this is not suitable for LSMT.
- A cost-model. So no need to re-tune model for different datasets.