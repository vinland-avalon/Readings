
# What problem is the paper trying to solve?

Indexes are models. Range indexes like B-trees are given key then output offset of data on a sorted dataset. Point indexes like hashmap do the same thing on an unsorted dataset. Existence indexes like Bloom filter test whether an element is a member of a set. 
Such indexes are normally implemented in a general way, through specific data structures and algorithms. However, since they can handle all data equally, they assume nothing about the data distribution and do not take advantage of more common patterns prevalent in real world data. For example, if we want to do search on a set of continuous integer keys (e.g., the keys 1 to 100M), a better choice is to just calculate offset by multiply key by key size, rather than a conventional B-Tree.

# Why is the problem important?

If we can utilize the data distribution, among which is CDF, there might be optimization on both time and space complexity. Take the "continuous integer keys" dataset as an example again. It can then reach O(1) complexity. 

# What sets it apart from prior work?

Unlike traditional indexing methods that do not adapt to the specific data they are indexing, learned indexes utilize the actual distribution of the data. This is a paradigm shift from a one-size-fits-all model to a tailored approach that learns from the dataset's unique characteristics.  
Such distribution is grapped with the novel use of machine learning models, specifically neural networks, as a replacement for core components of indexing structures. 

# What are the key technical ideas?

## Range Indexes (Normally B-tree for traditional indexes)
### Modeling the Cumulative Distribution Function (CDF)
In the context of a sorted array, a range index can be viewed as estimating the CDF of the keys in the dataset: it maps a key to a position with a min- and max-error Then we can use a regression model with error gurantee to replace B-tree.
### Optimization on A First, Naïve Learned Index
To test the performance, the authors built a "First, Naïve Learned Index" and find it is not as good as expected. The they proposed the following approach to optimize.
- **Limitation of frameworks like Tensorflow** - **The Learning Index Framework (LIF)**  LIF generates different index configurations, optimizes them, and tests them automatically. It will extract all weights from the model and generates efficient index structures in C++ thus can achieve faster calculation.
- **"Last mile" problem for search** - **The Recursive Model Index** It is a hierarchical structure of machine learning models. The RMI starts with a top-level model that makes a coarse prediction about the position of a key. This prediction then determines which second-level model to consult for a more refined prediction, and this process can continue for several layers. The final prediction pinpoints a narrow range within the dataset where the key is likely to be found. Also, since it is hierarchical, the models of different level and different node do not need to be the same. A B-tree can even used at leaf node for dataset with complex data distribution.

## Point Indexes (Normally Hashmap for traditional indexes)
This paper proposed a learned hash function aimed to avoid conflicts. The learned hash function effectively acts by mapping keys to their cumulative distribution function (CDF) values, scaled by the hash table size. This mapping predicts the position of a key in the sorted order of the dataset, which, when scaled, determines the bucket in the hash table where the key is likely to be found.

## Existence Indexes (Normally Bloom filters for traditional indexes)
Existence indexes like Bloom filters are to predict whether a key exists in the dataset, guaranteeing no false negatives and a configurable false positive rate.   
An alternative approach discussed is to learn a hash function that maximizes collisions among similar items (either both keys or both non-keys) while minimizing collisions between keys and non-keys. 
One way to frame the existence index is as a binary probabilistic classification task. There are many researches on how to implement such a classifier. The authors then append an overflow Bloom filter to gurantee no false negatives.

# What are the main areas of improvements and open questions?
- **Exploring Other ML Models**  The authors mainly used linear models and neural networks, including those with a mixture of experts. However, there are many other types of machine learning models and interesting ways they could be combined with classic data structures.
- **Multi-Dimensional Indexes** The distribution of primary key or the sort key is easier to grasp than other keys. However, to implement a multi-dimensional index, distribution of all keys is a must. There should be more research on how to design such indexes.
- **Learned Algorithms** An interesting thing is, distribution patterns, including CDF models have also the potential to speed-up sorting and joins, not just indexes. So it may inspire us to develop something like "learned algorithms".


