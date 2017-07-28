---
title: PythonSpeed
date: 2017-07-28 13:55:14
tags:
- python
- speed_up
categories: code
---
Python提速的学习笔记
<!-- more -->

# PythonSpeed

## Use the best algorithms and fastest tools

- Membership testing with sets and dictionaries is much faster, O(1), than searching sequences, O(n). When testing "a in b", b should be a set or dictionary instead of a list or tuple.


```python
import random
import operator
import numpy as np
import collections
```


```python
a = list(range(100))
b = set(a)
c = random.randint(0,200)
```


```python
%timeit c in a
%timeit c in b
```

    1.28 µs ± 47.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    55.5 ns ± 2.38 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


- String concatenation is best done with ''.join(seq) which is an O(n) process. In contrast, using the '+' or '+=' operators can result in an O(n**2) process because new strings may be built for each intermediate step. The CPython 2.4 interpreter mitigates this issue somewhat; however, ''.join(seq) remains the best practice.


```python
y="YYYY"
m="MM"
d="DD"
```


```python
%timeit y+"-"+m+"-"+d
%timeit "-".join((y,m,d))
```

    293 ns ± 8.29 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    199 ns ± 4.9 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)


- Many tools come in both list form and iterator form (range and xrange, map and itertools.imap, list comprehensions and generator expressions, dict.items and dict.iteritems). In general, the iterator forms are more memory friendly and more scalable. They are preferred whenever a real list is not required. 
(py2 only)

- Many core building blocks are coded in optimized C. Applications that take advantage of them can make substantial performance gains. The building blocks include all of the builtin datatypes (lists, tuples, sets, and dictionaries) and extension modules like array, itertools, and collections.deque.

- Likewise, the builtin functions run faster than hand-built equivalents. For example, map(operator.add, v1, v2) is faster than map(lambda x,y: x+y, v1, v2).


```python
v1=list(range(100))
v2=list(range(120))
```


```python
%timeit map(operator.add, v1, v2)
%timeit map(lambda x,y: x+y, v1, v2)
```

    263 ns ± 7.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    302 ns ± 11.8 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)


- Lists perform well as either fixed length arrays or variable length stacks. However, for queue applications using pop(0) or insert(0,v)), collections.deque() offers superior O(1) performance because it avoids the O(n) step of rebuilding a full list for each insertion or deletion.

- Custom sort ordering is best performed with Py2.4's key= option or with the traditional decorate-sort-undecorate technique. Both approaches call the key function just once per element. In contrast, sort's cmp= option is called many times per element during a sort. For example, sort(key=str.lower) is faster than sort(cmp=lambda a,b: cmp(a.lower(), b.lower())).

## Take advantage of interpreter optimizations

- In functions, local variables are accessed more quickly than global variables, builtins, and attribute lookups. So, it is sometimes worth localizing variable access in inner-loops. For example, the code for random.shuffle() localizes access with the line, random=self.random. That saves the shuffling loop from having to repeatedly lookup self.random. Outside of loops, the gain is minimal and rarely worth it.

- The previous recommendation is a generalization of the rule to factor constant expressions out of loops. Likewise, constant folding needs to be done manually. Inside loops, write "x=3" instead of "x=1+2".

- Function call overhead is large compared to other instructions. Accordingly, it is sometimes worth in-lining code inside time-critical loops.

- List comprehensions run a bit faster than equivalent for-loops (unless you're just going to throw away the result).

- Starting with Py2.3, the interpreter optimizes while 1 to just a single jump. In contrast, prior to Python 3, while True took several more steps. While the latter was preferred for clarity, time-critical code should have used the first form. Starting in Python 3, True, False, and None are reserved words, so there is no longer any performance difference here. See WhileLoop for additional details.

- Multiple assignment is slower than individual assignment. For example "x,y=a,b" is slower than "x=a; y=b". However, multiple assignment is faster for variable swaps. For example, "x,y=y,x" is faster than "t=x; x=y; y=t".


```python
%timeit x,y=1,2
%timeit x=1; y=2
```

    29.4 ns ± 0.51 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
    27.2 ns ± 0.745 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


- Chained comparisons are faster than using the "and" operator. Write "x < y < z" instead of "x < y and y < z".

- A few fast approaches should be considered hacks and reserved for only the most demanding applications. For example, "not not x" is faster than "bool(x)".


```python
%timeit bool(1)
%timeit not not 1
```

    111 ns ± 5.2 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
    28.9 ns ± 1.86 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


## References

[1] [PythonSpeed](https://wiki.python.org/moin/PythonSpeed)
