---
title: Compile Time String Hashing
date: 2016-11-16 00:12:21
tags: [C++11,c++14,Hashing]
mathjax: true
toc: true
---
## why do we need it
String comparison is a relatively slow operation and some times not tolerable. Especially when you have many if..else.. (or switch/case) code like this:  
if (mode == "StateA") {  
...  
}  
else if (mode == "StateB") {  
  ...  
}

The first thought come in mind is to use hash function to speed up the comparison operation; so we can improve the time complexity from O($n^2$) to O(1). How can we do better? Make the hash calculation on compile time so there will be no run-time cost and let the compiler do the job for you.

## c++98 solutions
Before c++11, c++98 solutions are not perfect. 
Such as:  

* [quasi compile time string hashing](http://www.gamasutra.com/view/news/127915/InDepth_Quasi_CompileTime_String_Hashing.php)  
Cons: This solution does not work for "switch/case" statements.  
* [using Boost.MPL](http://arcticinteractive.com/2009/04/18/compile-time-string-hashing-boost-mpl/)  
Cons: This solution needs to separate strings into groups.

## c++11/c++14 solutions
In c++11/c++14, constexpr provides us a new way to the perfect solution for compile time string hashing. However different c++ compilers support the new standard differently and we still need to make sure the code is fully tested on multiple compilers.

So first let's choose some popular Hash functions to start.




  