---
title: Lock Free Ring Queue for Multi-producer and Multi-consumer
tags: [C++11,lock-free,atomics,Dpdk,LMax Disruptor]
---
# Lock-free Ring Queue - What is it?
In a nutshell: it is a inter-thread messaging data structure. 

## A little history
* It brought a lot of attention in the concurrency world from the name - LMAX Disruptor.
You can still find the presentation back on Dec 2010 [here](https://www.infoq.com/presentations/LMAX). Never heared of Disruptor? Even spring framework uses it undernearth since [2013](https://spring.io/blog/2013/11/12/it-can-t-just-be-big-data-it-has-to-be-fast-data-reactor-1-0-goes-ga).

* I then found at the core of Disruptor, the lock-free ring queue was there in BSD code back in [2008](https://svnweb.freebsd.org/base/head/sys/sys/buf_ring.h?view=markup&pathrev=185162). 

* The same data structure is used in [linux kernel](http://lxr.free-electrons.com/source/include/linux/kfifo.h).
* Also in famous intel [DPDK framework](http://dpdk.org/browse/dpdk/tree/lib/librte_ring/rte_ring.h).  


Who is the original author of the lock-free Ring Queue still remain unknown but as you can see this is an awesome data structure used by some most trust-worth projects.  

# Why redo the wheel in C++11 ?  
You might ask there are so many implementation available now, why do we redo it in c++11?  
Here is my humble opinion:
* The biggest benefit C++11 brought to us IMHO is multithreading memory model and atomics library in language level. Without it how much work you need to do to be confident to say your code will run correctly on different OS/CPU architecture. In the multi-core era if the language itself cannot address these issue what is the advantage to compete with Rust, Go, Clojure，Erlang, Haskell, Scala etc.





