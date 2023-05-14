# Basic Info
Title: [Tsunami: A Learned Multi-dimensional Index for Correlated Data and Skewed Workloads](https://dl.acm.org/doi/10.14778/3425879.3425880)
# First Impression
- Extend of [Flood](https://arxiv.org/abs/1912.01668)[1]
- *skewed query* A lightweight decision tree, named `Grid tree`, to partition better
- *correlated dimensions* `Augmented tree`, which use two techniques - *functional mappings* and *conditional CDFs*
# How does it work
## Pre-requisite
### Flood
Flood is a in-memory multi-dimensional index that automatically optimizes its structure to achieve high performance on a particular dataset and workload.
### Skewed Query Workloads
For example, 90% queries are hitting 1% data, then they are.
