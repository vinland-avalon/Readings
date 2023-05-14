# Basic Info
Title: [Learning Multi-dimensional Indexes](https://arxiv.org/pdf/1912.01668v1.pdf)
# First Impression
- A multi-dimensional read-optimized index
- At the expense of writes
- Adapt itself to a particular *dataset* and *workload* jointly with analyzer
- Optimize both index structure (most likely to Grid files) and data storage layout
## Inspiration
- It uses *sample filter workload* to learn how often certain dimensions are used, which ones are used together, and which are more selective. Then according to such information, customize the data layout.
- 
# How does it work
## Query Operation
![Flood_Query](https://github.com/vinland-avalon/Readings/blob/main/images/Flood_Query.png?raw=true)
Attention: the data inside cell is only sorted by dth-dimension.
