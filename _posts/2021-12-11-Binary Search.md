---
layout: post
title: Binary Search
date: 2021-12-11 13:10:00
author: "phjung1"
header-img: "#"
tags:

- Algorithms
- Searching and Sorting
- Binary Search

---

# Binary Search

Given1 a sorted array arr[] of n elements, write a function to search a given element x in arr[].  

A simple approach is to do a [**linear search**](https://phjung1.github.io/2021/12/11/Linear-Search/)**.** The time complexity of the above algorithm is O(n). Another approach to perform the same task is using Binary Search.   

**Binary Search:** Search a sorted array by repeatedly dividing the search interval in half. Begin with an interval covering the whole array. If the value of the search key is less than the item in the middle of the interval, narrow the interval to the lower half. Otherwise, narrow it to the upper half. Repeatedly check until the value is found or the interval is empty.

![](https://www.geeksforgeeks.org/wp-content/uploads/Binary-Search.png)

The idea of binary search is to use the information that the array is sorted and reduce the time complexity to O(Log n).

We basically ignore half of the elements just after one comparison.

1. Compare x with the middle element.

2. If x matches with the middle element, we return the mid index.

3. Else If x is greater than the mid element, then x can only lie in the right half subarray after the mid element. So we recur for the right half.

4. Else (x is smaller) recur for the left half.

**Recursive** implementation of Binary Search

```cpp
// C++ program to implement recursive Binary Search
#include <bits/stdc++.h>
using namespace std;

// A recursive binary search function. It returns
// location of x in given array arr[l..r] is present,
// otherwise -1
int binarySearch(int arr[], int l, int r, int x)
{
    if (r >= l) {
        int mid = l + (r - l) / 2;

        // If the element is present at the middle
        // itself
        if (arr[mid] == x)
            return mid;

        // If element is smaller than mid, then
        // it can only be present in left subarray
        if (arr[mid] > x)
            return binarySearch(arr, l, mid - 1, x);

        // Else the element can only be present
        // in right subarray
        return binarySearch(arr, mid + 1, r, x);
    }

    // We reach here when element is not
    // present in array
    return -1;
}

int main(void)
{
    int arr[] = { 2, 3, 4, 10, 40 };
    int x = 10;
    int n = sizeof(arr) / sizeof(arr[0]);
    int result = binarySearch(arr, 0, n - 1, x);
    (result == -1)
        ? cout << "Element is not present in array"
        : cout << "Element is present at index " << result;
    return 0;
}
```

**Output :**

Element is present at index 3

Here is recursive implementation with check function which I feel is a much easier implementation:

```cpp
#include <bits/stdc++.h>
using namespace std;

// define array globally
const int N = 1e6 + 4;

int a[N];
int n; // array size

// element to be searched in array
int k;

bool check(int dig)
{
    // element at dig position in array
    int ele = a[dig];

    // if k is less than
    // element at dig position
    // then we need to bring our higher ending to dig
    // and then continue further
    if (k <= ele) {
        return 1;
    }
    else {
        return 0;
    }
}
void binsrch(int lo, int hi)
{
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (check(mid)) {
            hi = mid;
        }
        else {
            lo = mid + 1;
        }
    }
    // if a[lo] is k
    if (a[lo] == k)
        cout << "Element found at index "
            << lo; // 0 based indexing
    else
        cout
            << "Element doesnt exist in array"; // element
                                                // was not in
                                                // our array
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    cin >> k;

    // it is being given array is sorted
    // if not then we have to sort it

    // minimum possible point where our k can be is starting
    // index so lo=0 also k cannot be outside of array so end
    // point hi=n

    binsrch(0, n);

    return 0;
}
```

**Iterative** implementation of Binary Search

```cpp
// C++ program to implement recursive Binary Search
#include <bits/stdc++.h>
using namespace std;

// A iterative binary search function. It returns
// location of x in given array arr[l..r] if present,
// otherwise -1
int binarySearch(int arr[], int l, int r, int x)
{
    while (l <= r) {
        int m = l + (r - l) / 2;

        // Check if x is present at mid
        if (arr[m] == x)
            return m;

        // If x greater, ignore left half
        if (arr[m] < x)
            l = m + 1;

        // If x is smaller, ignore right half
        else
            r = m - 1;
    }

    // if we reach here, then element was
    // not present
    return -1;
}

int main(void)
{
    int arr[] = { 2, 3, 4, 10, 40 };
    int x = 10;
    int n = sizeof(arr) / sizeof(arr[0]);
    int result = binarySearch(arr, 0, n - 1, x);
    (result == -1) ? cout << "Element is not present in array"
                : cout << "Element is present at index " << result;
    return 0;
}
```

**Output :**

Element is present at index 3

**“Bitwise binary search”**

  Idea:

Every number can be represented as a sum of the powers of the number 2.

Exemples:

1. 76 = 64 + 8 + 4
2. 10 = 8  + 2
3. 7 = 4 + 2 + 1

**Approach:**

Compute the first power of 2 that is greater or equal then the size of the array.

Initialize an index as 0.

Loop while the computed power is greater than 0 and each time divide it by 2.

Each time the element at position [index + power] <= target we add to the index variable the respective power value. (Build the sum)

After the for loops check if the element at position [index] == target. If so the target element is present in the array, else not.

(no division needed only addition and bitwise shifting)

```cpp
// C++ program to implement Bitwise Binary Search

#include<iostream>

using namespace std;

int binary_search(int *arr,int size,int target)
{
    int index, power;

    //Compute the first power of 2 that is >= size
    for (power = 1; power < size; power <<= 1);

    //loop while(power > 0)
    //and divide power by two each iteration
    for (index = 0; power; power >>= 1){

        //if the next condition is true
        //it means that the power value can contribute to the "sum"(a closer index where target might be)
        if (index + power < size && arr[index + power] <= target)
        index += power;


    }

    //if the element at position [index] == target,
    //the target value is present in the array
    if(arr[index] == target)
        return index;

    //else the value is not present in the array
    return -1;
}

int main(){
    int arr[5] = {1, 3, 5, 7, 8};
    int size = 5;
    int x = 3;
    int answer = binary_search(arr, size, x);
    if(answer == -1)
        cout << "Element not found";
    else
        cout << "Element found at position " << answer;
}
// This code is contributed
// by Gatea David
```

**Time Complexity:**

The time complexity of Binary Search can be written as:

$$
T(n) = T(\frac{n}{2})+c
$$

The above recurrence can be solved either using the Recurrence Tree method or the Master method. It falls in case II of the Master Method and the solution of the recurrence is ![Theta(Logn)](https://www.geeksforgeeks.org/wp-content/ql-cache/quicklatex.com-78d402ebac9d0db6acf4763cb925d05e_l3.svg "Rendered by QuickLaTeX.com").  
**Auxiliary Space:** O(1) in case of iterative implementation. In the case of recursive implementation, O(Logn) recursion call stack space.  
**Algorithmic Paradigm:** [Decrease and Conquer](https://www.geeksforgeeks.org/decrease-and-conquer/).

**Note:**

Here we are using

$$
mid = low + \frac{high - low}{2}
$$

Maybe, you wonder why we are calculating the ***middle index*** this way, we can simply add the *lower and higher index and divide it by 2.*

$$
mid = \frac{low+high}{2}
$$

But if we calculate the ***middle index*** like this means our code is not 100% correct, it contains bugs.

That is, it fails for larger values of int variables low and high. Specifically, it fails if the sum of low and high is greater than the maximum positive int value

$$
2^{31}-1
$$



The sum overflows to a negative value and the value stays negative when divided by 2. In java, it throws *ArrayIndexOutOfBoundException.*

$$
mid = low + \frac{high - low}{2}
$$

So it’s better to use it like this. This bug applies equally to merge sort and other divide and conquer algorithms.
