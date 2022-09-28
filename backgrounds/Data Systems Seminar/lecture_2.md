---
layout: page
title: "Data Systems Seminar - Lecture 2"
author: "Donghyun Sohn"
date: "2022/9/22"
tags:
  - DBMS
use_math: true
---

## Notice

This posting is based on Prof. Andrew Crotty's <b>Northwestern CS496 Special Topics In Data Systems (Fall 2022)</b> lecture. <br>

Lecture link : [https://github.com/crottyan/cs496-f22](https://github.com/crottyan/cs496-f22)

<br>

## B+ Tree
### Introduction
A B+Tree is a self-balancing tree data structure that keeps data sorted and allows searches, sequential access, insertions, and deletions always in O(logn)
- Generalization of a binary search tree, since a node can have more than two children
- Optimized for systems that read and write large blocks of data

### Properties
A B+Tree is an M-way search tree with the following properties :
- It is perfectly balanced
- Every node other than the root is at least half-full : M/2-1 <= #keys <= M-1
- Every inner node with k keys has k+1 non-null children

### Duplicate keys
1. Append Record ID
   - add the tuple's unique record ID as part of the key to ensure that all keys are unique
   - the DBMS can still use partial keys to find tuples
2. Overflow Leaf Nodes
   - allow leaf nodes to spill into overflow nodes that contain the duplicate keys
   - this is more complex to maintain and modify

### Design choices
1. Node Size
   - the slower the stroage device, the larger the optimal node size for a B+Tree
2. Merge Threshold
   - some DBMSs do not always merge nodes when they are half full
   - delaying a merge operation may reduce the amount of reorganization
3. Variable-Length Keys
   - pointers
   - variable-length nodes
   - padding
4. Intra0Node Search


## LSM Tree
Disk-based data structure but they keep subset in the memory, and the idea is that going to use that for low-cost indexing for high ingest insert/delete volume. <br>
<b> Worse Read / Better Write  </b>

### 5 Min Rule
Rule of thumb presented by Jim Grey<br>
DB Pages that we reference every 5 mins should always be memory resident <br>
