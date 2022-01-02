---

layout:     post
title:      Performance of loops (A caching question)
date:       2021-12-11 12:38:00
author:     "phjung1"
header-img: "#"
tags:
    - Algorithms
    - Analysis of Algorithms

---

# Performance of loops (A caching question)

Consider below two C language functions to compute sum of elements in a 2D array. Ignoring the compiler optimizations, which of the two is better implementation of sum?

```c
// Function 1
int fun1(int arr[R][C])
{
    int sum = 0;
    for (int i=0; i<R; i++)
      for (int j=0; j<C; j++)
          sum += arr[i][j];
}

// Function 2
int fun2(int arr[R][C])
{
    int sum = 0;
    for (int j=0; j<C; j++)
      for (int i=0; i<R; i++)
          sum += arr[i][j];
}
```

In C/C++, elements are stored in Row-Major order. So the first implementation has better spatial locality (nearby memory locations are referenced in successive iterations). Therefore, first implementation should always be preferred for iterating multidimensional arrays.
