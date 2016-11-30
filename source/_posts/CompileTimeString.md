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

So first let's first pick up an easy Hash function to start. The trick is to utilize the c++11


* template/macro (two ways for compile time processing) 
* constexpr syntax  
 
to pre-calculate the hash value and sometimes with the help of template recursion in compile time.

#### Example A
Java string's hashCode() is one of the simplest hash function which uses [Horner's method](https://en.wikipedia.org/wiki/Horner's_method):
$$h = s[0] * 31^\left(n-1\right)+s[1] * 31^\left(n-2\right) + ... + s[n-1]$$


It's not hard to deduce the following iterative equation:  


$$h = h * 31 + s[n-1]$$

Based on this recusive nature you can write a simple recursive c++ function:  
```
template <size_t N>
constexpr inline size_t HORNER_HASH(size_t prime, const char (&str)[N], size_t Len = N-1)
{
	return (Len <= 1) ? str[0] : (prime * HORNER_HASH(prime, str, Len-1) + str[Len-1]);
}
#define CompileTimeHash1(x) (HORNER_HASH(31, x))
```  
If you are not familiar with syntax const ```const char (&str)[N]``` Stefan Reinalter gave an explanation [here](https://blog.molecular-matters.com/2011/06/22/subtle-differences-in-c/).Note ```N``` is deduced to be ```sizeof(str)``` which including the null terminator for c string so we have to pass ```N-1``` as the length of c string.

The following testing code validates if the runtime hash function generates the same result as the compile time hash function both in ```if``` and ```switch``` statements. 
```
#include <stdio.h>

template <size_t N>
constexpr inline size_t HORNER_HASH(size_t prime, const char (&str)[N], size_t Len = N-1)
{
	return (Len <= 1) ? str[0] : (prime * HORNER_HASH(prime, str, Len-1) + str[Len-1]);
}

#define COMPILE_HORNER_HASH(x) (HORNER_HASH(31, x))

size_t RUNTIME_HORNER_HASH(size_t prime, char const* str)
{
    if (str == NULL) return 0;
	size_t hash = *str;
    for (; *(str+1)!=0; str++)
    {
      hash = prime * hash + *(str+1);
    }
    return hash;
}

int main()
{
    const char* currentState = "StateA";
    if (RUNTIME_HORNER_HASH(31,currentState) == COMPILE_HORNER_HASH("StateA"))
    	printf("if statement test success!\n");
    switch (RUNTIME_HORNER_HASH(31,currentState))
    {
    case COMPILE_HORNER_HASH("StateA"):
    printf("switch statement test success!\n");
    break;
    }
    return 1;
}
```




  