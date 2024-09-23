# [SAP HANA](http://sites.computer.org/debull/A12mar/hana.pdf)
## Architecture
![hana-architecture](https://github.com/vinland-avalon/Readings/blob/main/images/hana-architecture.png?raw=true)
In-memory delta + in-memory main storage + unload
## Inspirations
### Column Store absolutely works well for OLAP. BUt sometimes it even works well for OLTP.
Especially for some ERP scenarios, because:
- Compression schemes.
- Compared to TCP-C, real world transaction workload has more read
- "Append-only" schema is more efficient than in-place updates sometimes.
- Column-stores greatly reduce the need for indices: As a matter of fact, the high scan performance of column-stores on modern hardware permits us to have indices only for primary keys, columns with unique constraints and frequent join columns.
### Calculation Model for SQLScript and other interfaces