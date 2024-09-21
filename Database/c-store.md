# C-Store: A Column-oriented DBMS

## Innovation Features
The C-Store physical DBMS design problem is to determine the collection of projections, segments, sort keys, and join indices to create for the collection of logical tables in a database.  
1. A hybrid architecture with a WS component optimized for frequent insert and update and an RS component optimized for query performance. (The tuple mover is like "merge out process" of LSM-tree).
![c-store-architecture](https://github.com/vinland-avalon/Readings/blob/main/images/c-store-architecture.png?raw=true)
2. Redundant storage of elements of a table in several overlapping projections in different orders, so that a query can be solved using the most advantageous projection.
3. Heavily compressed columns using one of several coding schemes.
4. A column-oriented optimizer and executor, with different primitives than in a row-oriented system.
5. High availability and improved performance through K-safety using a sufficient number of overlapping projections.
6. The use of snapshot isolation to avoid 2PC and locking for queries. (for read-only queries).
## Inspired
Correction of my previous assumption. Column store no need to store every column as a projection, and the sort function no need to be the same. Instead, there could be multiple, like:
```
# Projections in with sort orders
EMP1(name, age| age)
EMP2(dept, age, DEPT.floor| DEPT.floor) 
EMP3(name, salary| salary) 
DEPT1(dname, floor| floor)
```
And thus **Join Indices" are needed, which bridges different projections:
![c-store-join-indices](https://github.com/vinland-avalon/Readings/blob/main/images/c-store-join-indices.png?raw=true)
If there are multiple projections, maybe need a path of several join indices to re-construct the original table.