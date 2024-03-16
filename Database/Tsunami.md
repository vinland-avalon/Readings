
# What problem is the paper trying to solve?

Multi-dimensional queries refer to those queries whose "where clause" involves several fields and are normal in DBMS. There are both traditional ways (like k-d tree) and learned index ways (like Flood) to deal with it. However, both of them have limitations.  
## Traditional Multi-dimentional Index (like k-d tree)
They are hard to tune and require an admin to carefully pick which dimensions to index. And this decision must be revisited every time the data or workload changes.
## Learned Multi-dimentional Index (like Flood)
Flood divides each dimension into some number of partitions based on the observed data and workload and thus comprises *grids*. Such optimization for data layout is based on workloads and data distribution. However, it still faces two lmitations, namely **skewed query workloads and correlated dimensions**.

# Why is the problem important?

Such two scenarios are normally seen. So how to optimize learned multi-dimensional indexes while keep their advantages is important.
- **Query skew Queries** Queries often hit recent data more frequently than stale data, and operations monitoring systems only query for health metrics that are exceedingly low or high.
- **Correlations** The price and distance of a taxi ride are correlated, as are the dates on which a package is shipped and received

# What sets it apart from prior work?

![Tsunami_vs_Flood](https://github.com/vinland-avalon/Readings/blob/main/images/Tsunami_vs_Flood.png?raw=true)
Figure 1 shows the Strengths (vs k-d tree) and Limitations (vs Tsunami) of Flood. The Tsunami is an optimized version of Flood. So now we mainly focus on the limitations.
- **Skewed Query Workloads** For example, in Fig. 1b there are a few queries in the lower-left region of the data space that, unlike the many queries in the upper-right region, have high selectivity over dimension X. Since these queries are a small fraction of the totalworkload, Flood’s optimization will not prioritize their performance. As a result, Flood will need to scan a large number of points to create the query result (red points in Fig. 1b).
- **Correlated Workloads** Flood’s model-based indexing technique can result in unequally-sized cells when data is correlated. In Fig. 1b, even though the CDF models guarantee that the three partitions over dimension X have an equal number of points, as do the six partitions over dimension Y, the 18 grid cells are unequally sized.

# What are the key technical ideas?

## Architecture
Two independent data structures: the Grid Tree to deal with skewed queries and the Augmented Grid to deal with data correlations. The Grid Tree is a space-partitioning decision tree that divides d-dimensional data space into some number of non-overlapping regions. In Fig. 1c, the Grid Tree divides data space into three regions by splitting on dimension X. Within each region, there is an Augmented Grid. Each Augmented Grid indexes the points that fall in its region.
## Grid Tree
Grid Tree is to divide the data space into a number of non-overlapping regions so that within each region, there is little query skew. The building of Grid tree is like this:
1. Group queries in the sample workload into some number of clusters, which we call *query types*, where each type has similar selectivity characteristics. For example, one type search uniformally and the other search recent data.
2. Build the Grid Tree in a greedy fashion. Each node will first choose dimension and then values to split. They defined *reduction in query skew* and try to maximum it with every nodes.
## Augmented Grid
An Augmented Grid is a grid in which each dimension X ∈[0,d) is dividedintopX partitions and uses one ofthree possible strategiesfor creating its partitions: 
1. Partition X independently of other dimensions, uniformly in CDF(X). This is what Flood does for every dimension. 
2. Remove X from the grid and transform query filters over X into filters over some other dimension Y ∈[0,d) using a *functional mapping F :X →Y*. Fuctional Mapping is suitable when a pair of dimensions X and Y is monotonically correlated if as values in X increase, values in Y only move in one direction. Error bounds also be applied here.
3. Partition X dependent on another dimension Y ∈[0,d), uniformly in CDF(X). Suitable for scenarios there is no tight monotonic correlations. Otherwise the error bounds will be too large.
![Tsunami_Augmented_Grid](https://github.com/vinland-avalon/Readings/blob/main/images/Tsunami_Augmented_Grid.png?raw=true)

# What are the main areas of improvements and open questions?
- Outliers. Functional mappings are not robust to outliers. One outlier could extend error bound to large scale.
- Multi-dimensional correlations. If X is correlated with both Y and Z, then simple Augmented Grid can not hanle.



