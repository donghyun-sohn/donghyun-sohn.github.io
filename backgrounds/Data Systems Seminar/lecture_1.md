---
layout: page
title: "Data Systems Seminar - Lecture 1"
author: "Donghyun Sohn"
date: "2022/9/20"
tags:
  - DBMS
use_math: true
---

## Notice

This posting is based on Prof. Andrew Crotty's <b>Northwestern CS496 Special Topics In Data Systems (Fall 2022)</b> lecture. <br>

Lecture link : [https://github.com/crottyan/cs496-f22](https://github.com/crottyan/cs496-f22)

<br>

## Key-Value Storage
### Introduction
Key-Value Storage is composed of key and value(=payload) <br>

It has 3 APIs

1. PUT : provide (k,v) / if already in there, replace it
2. DELETE : provide key, and delete associated value
3. GET : provide key, and get value

What this course about ? <br>
- figuring out what to put in this storage, to make these operators efficient both
- in terms of scale
- whole papers we're gonna read is what Data Structures / Algorithms / Techniques in here to make these operators efficient 

### Page-Oriented Architecture & Log-Structured Storage
#### Page-Oriented Architecture
DB systems traditionally assume that primary place to store data is disk. Last 50 years, people used page-oriented architecture. 
<br><br>
What are the problems of page-oriented architecture ? <br>
- Fragmentation
- Useless Disk I/O
- Random Disk I/O

#### Log-Structured Storage
DBMS stores log records that contain changes to tuples (PUT,DELETE)

<b>WRITE</b><br>
advantages compared to slooted pages 
- All disk writes are sequential
- On-disk pages are immutable

<b>READ</b><br>
scan log from newest to oldest<br>
maintain an index that maps a tuple id to the newest log record <br>
avoid random access <br><br>

In this case, the log will grow forever. The DBMS needs to periodically compact pages to reduce wasted space. <br>

compaction coalesces larger log files into smaller files by removing unnecessary records
1. universal compaction
2. level compaction -> levelDB
   
Log-structured storage managers are more common today. <br>
Downsides ? 
1. write-amplication : repeatedly R/W same key
2. compaction is expensive

#### Hash Tables

space complexity : O(n) <br>
time complexity : 
- average : O(1)
- worst : O(n)

We do not want to use a <b>cryptographic hash function</b> for DBMS hash tables. Because it has <b>high compute overhead</b> <br>

Hash Functions
- Google CityHash
- <b>Facebook XXHash</b> : state-of-the-art / fast and avoid collison
- Google FarmHash

Hashing Schems 
- Linear Probe Hashing
- Robin Hood Hashing
- Cuckoo Hashing

These three require the DBMS to know the number of elements it wants to store. Otherwise, it must rebuild the table if it needs to grow/shrink in size. <br>

Dynamic hash tables resize themselves on demand
- Chained Hashing
- Extendible Hashing
- Linear Hashing
  
Drawbacks of hash tables :<br>
<b>Only do point query, and can not do range query</b>
