# Disjoint Set Union
## Reference
- [cp-algorithms](https://cp-algorithms.com/data_structures/disjoint_set_union.html)
- [Leetcode-1202. Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/description/)
## Concepts
The *Disjoint Set Union (DSU)* is also called *Find Union* because of its two main operations. Once DSU should have at least two functions, which are all in O(1) time on average:
- `union(a, b)` - merges the two specified sets.
- `find(a)` - returns the highest leader of an element.
The DSU is a tree actually. In the following image you can see the representation of such trees for [[1,2],[3,4],[2,3]]
![DSU_example](https://github.com/vinland-avalon/Readings/blob/main/images/DSU_example.png?raw=true)
## Naive Implementation
```C++
void make_set(int v) {
    parent[v] = v;
}

int find_set(int v) {
    if (v == parent[v])
        return v;
    return find_set(parent[v]);
}

void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b)
        parent[b] = a;
}
```
## Find with Path Compression Optimization
first find the representative of the set (root vertex), and then in the process of stack unwinding the visited nodes are attached directly to the representative. O(logn) on average per call.(no proof here).
```C++
int find_set(int v) {
    if (v == parent[v])
        return v;
    return parent[v] = find_set(parent[v]);
}
```
## Union by size/rank
Attach the tree with the lower rank to the one with the bigger rank. `Rank` here refers to the depth of tree.
```C++
// union by size
void make_set(int v) {
    parent[v] = v;
    rank[v] = 0;
}

void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) {
        if (rank[a] < rank[b])
            swap(a, b);
        parent[b] = a;
        if (rank[a] == rank[b])
            rank[a]++;
    }
}
```
```C++
// union by size
void make_set(int v) {
    parent[v] = v;
    size[v] = 1;
}

void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) {
        if (size[a] < size[b])
            swap(a, b);
        parent[b] = a;
        size[a] += size[b];
    }
}
```
## Time Complexity
**Two functions are all in O(1) time on average.**
As mentioned before, if we combine both optimizations - path compression with union by size / rank - we will reach nearly constant time queries. It turns out, that the final amortized time complexity is  
$O(\alpha(n))$ , where  
$\alpha(n)$  is the inverse Ackermann function, which grows very slowly. In fact it grows so slowly, that it doesn't exceed  
$4$  for all reasonable  
$n$  (approximately  
$n < 10^{600}$ ).

Amortized complexity is the total time per operation, evaluated over a sequence of multiple operations. The idea is to guarantee the total time of the entire sequence, while allowing single operations to be much slower then the amortized time. E.g. in our case a single call might take  
$O(\log n)$  in the worst case, but if we do  
$m$  such calls back to back we will end up with an average time of  
$O(\alpha(n))$ .

We will also not present a proof for this time complexity, since it is quite long and complicated.

Also, it's worth mentioning that DSU with union by size / rank, but without path compression works in  
$O(\log n)$  time per query.

## Application
- Connected Components in graph
- Calculate the size of each connected components. Just like *Union by size*
- etc
