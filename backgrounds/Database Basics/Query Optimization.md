---
layout: page
title: "Query Optimization"
author: "Donghyun Sohn"
date: "2022/10/1"
tags:
  - DBMS
use_math: true
---


## Query Optimization

Finding query plan of minimum cost <br><br>
<b>How?</b> <br>
-> Need a way to measure cost of a plan (a cost model)
<br>

### Single table operations 

How do I compute the cost of a particular predicate ? <br>
-> <b> Compute it's selectivity : fraction F of tuples it passes </b> <br>

### Notation 

- NCARD(R) : relation cardinality -- number of tuples in R
- TCARD(R) : number of pages R occupies
- ICARD(I) : keys(distinct values) in index I
- NINDX(I) : pages occupied by index I
- min and max keys in indexes

### Estimating selectivity F

col = val <br>
<emsp> F = F = 1 / ICARD() (if index available) <br>
<emsp> F = 1/10 <br> <br>

col > val
<emsp> (max key - value) / (max key - min key) (if index available) <br>
<emsp> otherwise 1/3 <br><br>

col1 = col2
<emsp> 1/MAX(ICARD(col1,col2)) <br>
<emsp> otherwise 1/10 <br><br>


