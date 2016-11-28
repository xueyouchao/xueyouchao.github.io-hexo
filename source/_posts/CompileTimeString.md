---
title: Compile Time String
date: 2016-11-16 00:12:21
tags: C++11
mathjax: true
---

String comparison is a relative slow operation and some times not tolerable. Especially when you have many if..else.. (or switch case) code like this:

if (mode == "A1") {
...
} 
else if (mode == "A2") {
  ...
}

The first thought come in mind is to use hash function to speed up the comparison operation; so we can improve the time complexity from O($n^2$) to O(1).

