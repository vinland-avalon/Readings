# A Critique of ANSI SQL Isolation Levels
## Overview of all levels
![table](https://github.com/vinland-avalon/Readings/blob/main/images/Critique_table.png?raw=true)
Notice:
- Cursor Stability: For reads, lock moves as cursor moves. For update, once currenet row is updated, hold lock until commit. Can prevent P4C but not P4.
- P4 (lost update): r1[x]...w2[x]...w1[x]...c1
- P4C (cursor lost update): rc1[x]...w2[x]...w1[x]...c1
- The reason for "P3(phantom) + Snapshot Isolation" to "*sometimes* possible": can prevent A3 (strict) but not P3 (broad).
- SI can prevent P4(lost update): first-committer-wins.
- ANSI SQL intended to define REPEATABLE READ isolation to exclude all anomalies except Phantom. The anomaly definition of Table 1 does not achieve this goal, but the locking definition of Table 2 does.
![tree_view](https://github.com/vinland-avalon/Readings/blob/main/images/critique_tree_view.png?raw=true)