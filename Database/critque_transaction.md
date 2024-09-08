# A Critique of ANSI SQL Isolation Levels
## Contribution
- New defination of **phenomenon** and **anomply**. The former one are translated as broad isolations, which are better.
![Px](https://github.com/vinland-avalon/Readings/blob/main/images/Critique_Px.png?raw=true)
- How **locking isolation** correspond to these new definations.
- **Snapshot Isolation**.
## Overview of all levels
![table](https://github.com/vinland-avalon/Readings/blob/main/images/Critique_table.png?raw=true)
Notice:
- **Ax** is for anomaly, strict interpretion, while **Px** is for phenomenon, which might not be an anomoly. For example:
```
For dirty reads:
P1: w1[x]...r2[x]...((c1 or a1) and (c2 or a2) in any order) 
A1: w1[x]...r2[x]...(a1 and c2 in any order)

The broad version is necessary cause sometimes none of strict definition is violated but anomoly shows up (while it violates P1). For example (transfer money):
H1: r1[x=50]w1[x=10]r2[x=10]r2[y=50]c2 r1[y=50]w1[y=90]c1
```
- For the typical 4 isolation levels: proposed version (which is defined by Px) == locking versions, while ANSI version (which is defined by Ax) is inadequate (not equal to locking version).
- The reason for "P3(phantom) + Snapshot Isolation" to "*sometimes* possible": can prevent A3 (strict) but not P3 (broad).
- SI can prevent P4(lost update): first-committer-wins.
![tree_view](https://github.com/vinland-avalon/Readings/blob/main/images/critique_tree_view.png?raw=true)

## Cursor Stability
To prevent the lost update phenomenon.
```
w1 update data item based on previous read, then update from w2 will lost.
P4: r1[x]...w2[x]...w1[x]...c1  (lost update)
P4C: rc1[x]...w2[x]...w1[x]...c1   (redefined Lost Update with rc and wc)
```
```
T1 to increase 30, T2 to increase 20, should be 150
r1[x=100] r2[x=100] w2[x=120] c2 w1[x=130] c1
```
Between READ COMMITTED and REPEATABLE READ. Implemented by locking behaviors applied to SQL cursor. For reads, lock moves as cursor moves. For update, once currenet row is updated, hold lock until commit. Can prevent P4C but not P4.  
Read Committed, in some systems, is actually the stronger Cursor Stability.  

## Snapshot Isolation
### First-committer-wins
The transaction successfully commits only if no other transaction T2 with a Commit-Timestamp in T1’s execution interval [Start- Timestamp, Commit-Timestamp] wrote data that T1 also wrote.  
P4 (Lost Update) is prevented.
### Why SI <<>> Repeatable Read
SI prevent P2 and A2 (non-repeatable read), however, A5B (write skew) could occur. RR is stronger.  
SI precludes A3 (has no phantoms, for strict definition). SI is stronger. However, worth noticing, SI does not preclude P3.
```
For example, according to predicate P (sum of work hours should be no greater than 8), T1 and T2 seperately insert different records and finally violate constraints together.
```