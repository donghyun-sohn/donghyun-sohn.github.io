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

## BigTable / LevelDB
### Reference : [Xiling Li's Presentation](https://github.com/crottyan/cs496-f22/blob/main/slides/bigtable.pdf)

### Motivation
- Large amount of data needed to be stored
- Data may be used to many different purposes
- Data may be stored in different machines

### What is LevelDB?
- Distributed storage system for structured data
- Key: (Row, Col, timestamp) ; Value : Arbitrary String

### Why LevelDB?
- Flexibility
  - For applications with various latency demands and data sizes
- Scalability
  - Petabyte-level data/thousand-level machines
- High performance/High availability
  - Thanks to Google building blocks

### Building Blocks
- Google File System (GFS)
  - Distributed log/data files storage
  - Managing scheduling, resources, failure and machine status
- SSTable file format
  - Persistent, ordered immutable map (key->value)
  - Quick lookup/key range scan with block index loaded into memory
- Chubby
  - Highly available and persistent distributed lock service
  - One active master server at any time

### Architecture
- Tablet
  - contains data with row range
- Tablet server
  - handle read/write requests to a set of tablets
- Master server
  - manage tablets (assignment, load balancing, garbage collection)
- Clients
  - Directly communicate with tablet servers

### Tablet in detail
- Memtable (Sorted)
  - Lexiographically sorted, new changes are stored in RAM
- SSTable (Sorted)
  - Persistently store old data
- METADATA table
  - Lists of SSTables and redo points
  - Recovery for memtable
- Read in the merged fashion

### Compaction
- Minor compaction (memtable size > a threshold)
  - create a new memtable
  - convert old one to SSTable and write to GFS
- Merging compaction
  - Periodically merge in the background to new SSTable
  - Discard old memtable and fetched SSTables after compaction
- Major compaction
  - Rewrite all SSTables in mergin fashion into one new SSTable
  - Get more space for deleted data if storage is sensitive
  
### Refinements
- Locality group 
  - Group (in-memory) multiple col families to make read faster
- Compression
  - Option to compress SSTables for a locality group
- Caching for read performance
  - Scan cache for k/v pairs from SSTable to tablet server
  - Block cache for SSTable blocks read from GFS
- Bloom filters
  - REduce # of access by creating BF for SSTable sin a locality group
- Commit-log implementation
  - Maintain a single physical log file
- Faster tablet recovery
  - Additional minor compaction for moving around tablets
- SSTable's Immutability
  - Efficient concurrency control

### Experiments
- Scan is faster than random read in memory
- write is faster than read in either sequential or random fashion (analogous to LSM-tree - fast write and slow read)

## Key-Value Store / Relational DB
KV storage is a lower level 
KV storage : Get / Put / Delete <br>
Relation Engine : SQL / Higher Query language 


## LevelDB Code Analysis
### Reference : [Yankai Jiang & Donghyun Sohn's Presentation](https://github.com/crottyan/cs496-f22/blob/main/slides/leveldb.pdf)

### Donghyun Sohn's script
Before addressing how the LevelDB operates, I will simply go through the high-level architecture of the LevelDB.
As we know, LevelDB gets the idea from LSM-Tree. 
So, I will talk about LevelDB architecture by comparing it to the LSM tree architecture. 

At first, I will talk about data structures residing in the memory. 
Tree like structure called C0 in the LSM-tree is separated by two memory tables. 
The name is Memtable, one is mutable and the other is immutable. These are managed by the data structure called Skiplist.

Next is the disk part. 
There are multi-level tree-like structures C1 to Ck in the LSM Tree. 
These are implemented similarly in the LevelDB, and the name is SSTable. They have multiple levels, and internally, these are managed by data block and index block. 

Since LevelDB is a Key-Value Store, we are using WAL when we write data. WAL stands for Write Ahead Log, and it writes every transaction of log in the LevelDB. The reason why we are using the WAL is that we do not want to lose the transaction. 

(Enter)

Also, LevelDB’s basic operations are put, delete, get. 
(Enter)
These are the codes of the basic operations. 
(Enter)
When we look at the features of the LevelDB, we can find out that data is stored sorted by key, and callers can customize comparison function. 
(Enter)
Therefore, it supports the forward and backward iteration, so leveldb can do the range scan. 

(Enter)

Now, I am going to talk about how the MemTable is works.
After the writes are appended to the log file, memtable stores the key value store. 
(Enter)

When the MemTable reaches the user defined write_buffer_size, a new Memtable is created and the original memtable becomes immutable.  
When the compaction happens during the background, then levelDB dumps the immutable table to the disk, so that sstable is created. 
(Enter)

When we look at these codes, we can infer the memtable’s key value entry. 
Key value entry consists of Sequence number, value type, userkey and value. 
LevelDB provides add and get operation but does not provide delete. If the deletion occurs, it just adds NotFound() key as one entry, and real deletion is held at compaction. 

(Enter)
MemTable uses Skiplist instead of B Tree as a data structure

Let me show you quick example of Skiplist through the wiki. 

Because it has several benefits. 

Random write) operation in a sorted structure is costly, which is usually where the bottleneck of performance is concentrated. B tree, skiplist, etc. are good to random write. Leveldb uses a lightweight skiplist, not a complex B tree.

Skiplist is a data structure that can replace binary trees. The skiplist can be balanced by probability (setting the level of node as a random function). The structure and implementation are simpler than binary trees, but the time complexity is not only similar O (logN) but also space saving.
 Updating from the binary tree can cause rebalancing, and change operations are expensive for bulk nodes. Therefore, the Skiplist has better concurrence performance than the binary tree:
There is no need to lock in the skiplist of LevelDB, because thread synchronization is done through the memory barrier:

(Enter)

SSTable has following structures. 

The first one is Data block.
 This is the block where key-value pairs are stored. 
Next block’s name is filter block, and we store bloom filter in here. 
Meta index block is block that stores index information of filter block

Index block is the block that have index information of each data blocks. 
And lastly, footer is the block that have index information of meta index block and index block. 
(Enter)
Now let’s see how to read . 

In the levelDB’s Get operation, we search the given key with the following orders. 

1. Search at MemTable
2. IF not, find at immutable memtable
3. if not, find at disk

(Enter)
Let’s look at how to find key in the stroage. 

1. At each level, select SS Tables that may have a target key
2. Find target key from picked SSTable

(Enter)
Through this Get method, we find target key from selected SSTable. 

1. Find if this SSTable is already cached, and if not cache this SSTable
2. Search internal of this SSTable to find target key. 

(Enter)
Through this internalGet method, we can find target key internally. 
1. Search the index block and find the data block that may have a target key
2. If using a Bloom Filter,  analyze if the selected data block has a target key through bloom filter
3. If concluded that there is a target key, make a iterator. 
4. use iterator to search data block
5. if target key found, store that value

(Enter)
Next, I will talk about the write situation of SSTable
Write happens in two situations.
1. Flush from memtable
2. compaction occurs at storage
(Enter)
I will address the first case
(Enter)
When background compaction happens it calls backgroundcompaction method and it keep calls through these arrors, and finally call buildtable function. 
BuildTable function actually make SSTable

(Enter)
In the BuildTable, it operates in the following orders. 

1. Create TableBuilder Instance
2. add memtable’s key-value pair to the tablebuilder
3. Finish adding
4. Write the contents to the storage
5. check if sstable is possible to put on the cache

(Enter)
I mentioned bloomfilter during the SStable. 
So I will briefly introduce bloom filter. 

Bloom filters are a stochastic data structure that can check whether a specific key of data exists in a data block.
When writing data, Bloom Filter performs k hash functions for each key to obtain k hash values for each key. And then change the value of the bloom filter from 0 to 1 to indicate that the corresponding key exists in the data block.
When you read, instead of reading all the blocks of data blocks of data, perform the same k hash function on the data you want to read to get k hash values
It is possible to improve read performance by selecting and reading only data blocks with all values of the corresponding array compartments of 1.
The bloom filter exists inside the SSTable’s filter meta block.
(Enter)
The code of Bloomfilter is written like this. 





