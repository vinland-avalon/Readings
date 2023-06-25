# HyperLogLog
An algorithm for the [count-distinct problem](https://en.wikipedia.org/wiki/Count-distinct_problem), which is to estimate number of distinct elements of a multiset.
## Reference
- [wikipedia](https://en.wikipedia.org/wiki/HyperLogLog)
- [demo](http://content.research.neustar.biz/blog/hll.html)
- [zhihu](https://zhuanlan.zhihu.com/p/77289303)
## How does it work
### Basic
The basis of the HyperLogLog algorithm is the observation that the cardinality of a multiset of uniformly distributed random numbers can be estimated by calculating the maximum number of leading zeros in the binary representation of each number in the set. If the maximum number of leading zeros observed is n, an estimate for the number of distinct elements in the set is $2^n$.
In the HyperLogLog algorithm, a hash function is applied to each element in the original multiset to obtain a multiset of uniformly distributed random numbers with the same cardinality as the original multiset.
### OPerations
**Add** to add a new element to the set, **Count** to obtain the cardinality of the set and **Merge** to obtain the union of two sets.
## Similar Algorithms
- HLL++: Proposes several improvements in the HyperLogLog algorithm to reduce memory requirements and increase accuracy in some ranges of cardinalities



