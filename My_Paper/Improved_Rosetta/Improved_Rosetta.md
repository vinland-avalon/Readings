# More Intelligent Rosetta: automatically adjust traverse order

- What is the motivation for this experiment or method you are suggesting? Why is this an important topic to research? What inspired you to propose this project?
- What are some existing works related to your proposed plan? Has anyone done something similar? Where do you think your proposal differs?

Modern data sets are large and grow at increasing rates. So efficient way to filter data, which helps to reduce scans, is more and more important these days. Database communnity has explored Bloom Filters (BFs) for many years and proposed many new innovations to adjust them to difference workloads. For example, Rosetta is one of the best version to handle point- and range-query in a unified way. A Rosetta instance uses a hierachical set of bloom filter. The k-level bloom filter is responsible for the probe of first k digits. During the range query, there will be a DFS-based traversal and whenever find positive result in a leaf node, it means the output of this range-qeury should be true.

It is good at short range query. However, for long range queries, which is more likely to contain target keys, other technologies such as SuRF outperforms. There are two reasons for the poor performance. First, for each query, only after reaching the most bottom leaf nodes can Rosetta the conclusion, while SuRF only make a conclusion according to the prefix. Second, once the current leaf indicates negative results, Rosetta will turn to next leaf node, until one leaf outputs positive or all leaves output negative. 

So this work will mainly focus on the second problem. It will apply other data structures to help with the traverse. More precisely, when choosing next level subtree to traverse, instead of pure DFS, the ones more likely to contain target keys will be given higher priority.

Such implemantation will include a Cumulative Distribution Function of the existing keys. Such CDF describes the possibility of keys of a specific range to appear within a sorted dataset. So during the traversal, for each non-leaf node, the first thing is to rank the subtrees. Such rank is according to the possiblity to contain keys which is calcualted through the CDF. We expect the improved model address the positive keys, if existed, as quickly as possible.
Also, piecewise CDF could also be applied here. It is derived from CDF and can use less parameter to describe data distribution with error bound.

For experiment, I'd like to first evaluate the lookups. I will compare orignal Rosetta, SuRF and Improved Rosetta. Such evaluation should take two independent variables, namely the "bits per key" and "size of range query". This experiment will evaluate time costs as well as False Positive Rate. 
Then I will conduct a second experiment to evaluate the storage cost. Compare to original Rosetta, we need to update an additional CDF model or piecewise CDF model for keys insertions and updates. The latter one should be more efficient to maintain.
Finally, I will conduct a set of experiments to verify how different paramenters will affect this system. For example, the size of CDF model.

What are the research questions that you would want to explore?
How do you plan to answer each of these questions listed in (3)? Think about how you will collect data, design the experiment or test your hypothesis.

# First Impression

## Inspiration
- 
- 
# How does it work
![segment-tree](https://github.com/vinland-avalon/Readings/blob/main/images/segment-tree1.png?raw=true)


