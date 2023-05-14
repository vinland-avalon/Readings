# How to deal with multi-dimensional indexs
The intersections of these ranges will define a hyper-rectangle.
## Traditional Solutions
- create **clustered indexes** over each single dimension
- create **multi-dimensional indexes** such as R-trees, k-d trees and oc-trees
- use **complex sort orders**, such as Z-ordering, which is used by Spark-SQL and RedShift
