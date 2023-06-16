# Floyd_Warshall Algorithm
## Reference
- [geeksforgeeks](https://www.geeksforgeeks.org/floyd-warshall-algorithm-dp-16/)
## How does it works
The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed Graph.  
It follows dynamic programming approach.  
The steps in details are as follows.
```python
Initialize the solution matrix same as the input graph edges matrix as a first step. 
Then update the solution matrix by considering all vertices as an intermediate vertex (transmit vertex). 
The idea is to one by one pick all vertices and updates all shortest paths which include the picked vertex as an intermediate vertex in the shortest path. 
When we pick vertex number i as an intermediate vertex, we already have considered vertices {0, 1, 2, .. i-1} as intermediate vertices. 
For every pair (j, k) of the source and destination vertices respectively, there are two possible cases. 
i is not an intermediate vertex in shortest path from j to k. We keep the value of dist[j][k] as it is. 
i is an intermediate vertex in shortest path from j to k. We update the value of dist[j][k] as dist[j][i] + dist[i][k] if dist[j][i] > dist[i][k] + dist[j][k]
```
### C++ implementation
```C++
// The `edges` is matrix whose size if of n*n, and could represent both bidirected graph and directed graph. If two vertexes are not connected directly, the edge should be initiated as big as possible. For example, 1e6+1, where we already know the max distance is at most 1e6. (You should be careful when use INT_MAX to avoid overflow).
vector<vector<int>> floyd(int n, vector<vector<int>>& edges){
    vector<vector<int>> diss(n, vector<int>(n));
    for(int i=0;i<n;i++) for(int j=0;j<n;j++) diss[i][j]=edges[i][j];
    // transit vertex
    for(int i=0;i<n;i++){
        // in-degree
        for(int j=0;j<n;j++){
            //out degree
            for (int k=0;k<n;k++){
                if(diss[j][i]+diss[i][k]<diss[j][k]){
                    diss[j][k]=diss[j][i]+diss[i][k];
                }
            }
        }
    }
    return diss;
}
```
## How to record the path
### Reference
- [zhihu](https://zhuanlan.zhihu.com/p/339542626)
- With another n*n path matrix to record the nearest transmit vertex.
