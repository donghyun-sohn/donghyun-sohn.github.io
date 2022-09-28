---
layout: page
title: "Data Systems Seminar - Lecture 3"
author: "Donghyun Sohn"
date: "2022/9/27"
tags:
  - DBMS
use_math: true
---

## Notice

This posting is based on Prof. Andrew Crotty's <b>Northwestern CS496 Special Topics In Data Systems (Fall 2022)</b> lecture. <br>

Lecture link : [https://github.com/crottyan/cs496-f22](https://github.com/crottyan/cs496-f22)

<br>

## LSM Tree

### Strengths 
- Low Disk I/O
- Lower Write Amplication
- Can get sequential write when you are doing write. 
  - sequential write is very important, especially for the disk-based I/O. But even in memory, sequential write is preferable to random access
- Less fragmentation between pages
- Can fetch the writes together, so to mortize the cost of having to do writes
- the rolling merge in particular allows you to recover efficiently from crashes
  - because you don't overwrite previous values. So you can roll back efficiently
- deleting with the tombstone values rather than physical deletes can help you avoid extra disk I/Os

### Weaknesses
- Allocate more resources for the compaction
- Space utilization
  - lower write amplication -> much more space

 ## Berkeley DB

 ### Embedded Database
 is a database management system (DBMS) which is tightly integrated with an application software; it is embedded in the application. It is actually a broad technology category that includes
 - database systems with differing application programming interfaces (SQL as well as proprietary, native APIs),
- database architectures (client-server and in-process),
- storage modes (on-disk, in-memory, and combined),
- database models (relational, object-oriented, entity–attribute–value model, network/CODASYL), and
- target markets

Reference : [https://en.wikipedia.org/wiki/Embedded_database](https://en.wikipedia.org/wiki/Embedded_database)

### Allow you to specify cuustom comparison / hash functions
In LSM Tree, sorted keys are key part. In the Berkeley-DB, extended this idea, you can provide custom comparison / hashing function

### Recommended Video
[https://www.youtube.com/watch?v=exoovHNk_54](https://www.youtube.com/watch?v=exoovHNk_54)



