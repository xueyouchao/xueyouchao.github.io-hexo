---
title: Lock Free Ring Queue for Multi-producer and Multi-consumer
tags: [C++11,lock-free,atomics,Dpdk,LMax Disruptor]
---
# Lock-free Ring Queue - What is it?
In a nutshell: it is a inter-thread messaging data structure. 

### A little history
* Lock-free ring queue datastructure attracted a lot of attention in the concurrency world because of the name LMAX Disruptor in [2011](http://www.oracle.com/us/corporate/press/512656).
You can still find the presentation back on Dec 2010 [here](https://www.infoq.com/presentations/LMAX). Never heared of Disruptor? Even spring framework uses it undernearth since [2013](https://spring.io/blog/2013/11/12/it-can-t-just-be-big-data-it-has-to-be-fast-data-reactor-1-0-goes-ga).

* I found at the core of Disruptor, the lock-free ring queue was actually in BSD code back in [2008](https://svnweb.freebsd.org/base/head/sys/sys/buf_ring.h?view=markup&pathrev=185162). 

* The same data structure is used in [linux kernel](http://lxr.free-electrons.com/source/include/linux/kfifo.h).
* Also in famous intel [DPDK framework](http://dpdk.org/browse/dpdk/tree/lib/librte_ring/rte_ring.h).  


Who is the original inventor of the lock-free Ring Queue data structure still remain unknown but as you can see this is an awesome data structure used by some most trustworthy projects.  

# Why redo the wheel in C++11 ?  
You might ask there are so many implementation available now, why do we redo it in c++11?  
Here is my humble opinion:
* The biggest benefit C++11 brought to us IMHO is its multithreading memory model and atomics library. Imagine without these two how much work do you need to do to make sure your multi-threading code will run correctly on different OS and CPU architecture. In the multi-core hardware era if c++ language itself cannot address compiler/cpu reordering and operation atomicity what is the advantage of c++ to compete with languages such as Rust, Go, Clojure，Erlang, Haskell, Scala etc. languages which are made for concurrency.  

# Talk is cheap, show me the code  
Here I will present a deadly simple lock-free MPMC(multi-producer, multi-consumer) ring queue implementation in c++11. The C implementation can be found at dpdk [git](http://dpdk.org/browse/dpdk/tree/lib/librte_ring)





