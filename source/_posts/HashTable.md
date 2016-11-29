---
title: HashTable and Hash Function
date: 2016-11-28 12:47
tags: [Algorithm,Hash]
mathjax: true
toc: true
---
## Hash Funciton Quick Recap
Hashtable is always like a magic data structure; providing you approximately O(1) time complexity for both insertion and searching operations in many situations. 
What makes this happen is the magic Hash function which map the keys into values as shown in the following Wiki image.  

![Hashing](https://upload.wikimedia.org/wikipedia/commons/7/7d/Hash_table_3_1_1_0_1_0_0_SP.svg)  
When you use Hashtable you can not ignore the hash collision issue. Jeff Preshing gave an excellent tutorial on the probabilities on the hash collision [here](http://preshing.com/20110504/hash-collision-probabilities/).
