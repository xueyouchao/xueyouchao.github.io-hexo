---
title: Start The Journey of Lock-Free Programming in C++11  
date: 2016-12-15 16:32:06  
tags: [C++11, Lock-Free]  
toc: true  
---
## What is & Why use Lock-Free Programming
To be added  

## Big Picture  
To be added  

## C++11 Atomics in Action  
To be added
### Building the Simplest SpinLock Using std::atomic_flag  
To be added

### Memory Ordering


### CAS in C++11  
C++11 provides two sets of API: OO style atomic class template & a set of C style functions. They are equivalent. The following demostrattion will based on atomic class template but you can easily convert to C style functions.

##### [exchange](http://en.cppreference.com/w/cpp/atomic/atomic/exchange)
```
T exchange( T desired, std::memory_order order = std::memory_order_seq_cst );
T exchange( T desired, std::memory_order order = std::memory_order_seq_cst ) volatile;
```
##### [compare_exchange_weak](http://en.cppreference.com/w/cpp/atomic/atomic/compare_exchange)
```
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order success, 
                            std::memory_order failure );
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order success, 
                            std::memory_order failure ) volatile;
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order order =
                                std::memory_order_seq_cst );
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order order =
                                std::memory_order_seq_cst ) volatile;  
```
##### [compare_exchange_strong](http://en.cppreference.com/w/cpp/atomic/atomic/compare_exchange)
```
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order success, 
                              std::memory_order failure );
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order success, 
                              std::memory_order failure ) volatile;
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order order = 
                                  std::memory_order_seq_cst );
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order order = 
                                  std::memory_order_seq_cst ) volatile;  
```










