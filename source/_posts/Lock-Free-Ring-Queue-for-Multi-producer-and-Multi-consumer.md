---
title: Lock Free Ring Queue for Multi-producer and Multi-consumer
tags: [C++11,lock-free,atomics,Dpdk,LMax Disruptor]
toc: true
---
## Lock-free Ring Queue - What is it?
In a nutshell: it is a inter-thread messaging data structure. 

## A little history
* Lock-free ring queue datastructure attracted a lot of attention in the concurrency world because of the name LMAX Disruptor in [2011](http://www.oracle.com/us/corporate/press/512656).
You can still find the presentation back on Dec 2010 [here](https://www.infoq.com/presentations/LMAX). Never heared of Disruptor? Spring framework uses it undernearth since [2013](https://spring.io/blog/2013/11/12/it-can-t-just-be-big-data-it-has-to-be-fast-data-reactor-1-0-goes-ga).

* The lock-free ring queue was sit at the core of Disruptor.  

* The same data structure was in BSD code back in [2008](https://svnweb.freebsd.org/base/head/sys/sys/buf_ring.h?view=markup&pathrev=185162).  

* The same data structure is used in [linux kernel](http://lxr.free-electrons.com/source/include/linux/kfifo.h).

* Also in famous intel [DPDK framework](http://dpdk.org/browse/dpdk/tree/lib/librte_ring/rte_ring.h).  

## Why redo the wheel in C++11 ?  
You might ask there are so many implementations available now, why do we redo it in c++11?  

* One of the biggest benefit C++11 brought to us IMHO is its multithreading memory model and atomics library. Imagine without these two how much work do you need to do to make sure your multi-threading code will run correctly on different OS and CPU architecture. In the multi-core hardware era c++11 brought us powerful weapons that enable both sequential consistency and weakly ordered multithreading models for our multithreading programming to squeeze our hardware's true power. We have a good start then we can build up better world to compete with Java, Rust, Go, Clojure,Erlang, Haskell, Scala etc. in this concurrent world.

## Talk is cheap, show me the code  
Here I will present a deadly simple lock-free MPMC(multi-producer, multi-consumer) ring queue implementation in c++11. The C implementation can be found at dpdk [git](http://dpdk.org/browse/dpdk/tree/lib/librte_ring).
Ring queue's documentation can be also found at dpdk [site](http://dpdk.org/doc/guides/prog_guide/ring_lib.html). 

In my simplest version, I have only implemented push/pop so far for an easy start; no bulk operations, no watermark notification, no dynamic size adjustments for the buffer.  
## Class Memebers
First let's look at ring queue class members:  

```
constexpr uint64_t RING_BUFFER_SIZE = 2 << 10;
constexpr uint64_t CACHE_LINE_SIZE = 64;

template <class T>
class ring_buffer_queue{

private:
	const static uint32_t size = RING_BUFFER_SIZE;
	const static uint32_t mask = size - 1;

	struct alignas(CACHE_LINE_SIZE) prod {
		std::atomic<uint32_t>  alignas(CACHE_LINE_SIZE) first;
		std::atomic<uint32_t>  alignas(CACHE_LINE_SIZE) second;
	}writer;

	struct alignas(CACHE_LINE_SIZE) consumer {
		std::atomic<uint32_t>  alignas(CACHE_LINE_SIZE) first;
		std::atomic<uint32_t>  alignas(CACHE_LINE_SIZE) second;
	}reader;

	simple_spin_wait spinlock;

	alignas(CACHE_LINE_SIZE) T buffer[size];
```

A few points to mention here:

* The fixed size of ring queue is usually power of 2, the reason for this is that you can use ```ring_pointer & (RING_BUFFER_SIZE-1)``` instead of ```ring_pointer % RING_BUFFER_SIZE``` for a pointer to circle back and point from the beginning of the ring queue.  
 
*  The prod/consumer pointers each contains first/second pointers (equivalent  to prod/consumer's head and tail in DPDK). The reason it needs first and second pointers is explained down below.

* ```alignas(CACHE_LINE_SIZE)``` used here is to prevent false sharing to happen. If you don't know what is false sharing, [here](https://nativecoding.wordpress.com/2015/06/19/multithreading-multicore-programming-and-false-sharing-benchmark/) is a simple and excellent explanation. Before c++11 this is done through padding chars, but now we can use new syntax ```alignas```.

* simple_spin_wait will be explained in a separate topic.







