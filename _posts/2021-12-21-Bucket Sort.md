---
layout: post
title: Bucket Sort
date: 2021-12-21 19:58:00
author: "phjung1"
header-img: "#"
tags:

- Algorithms
- Searching and Sorting
- Bucket Sort

---

# Bucket Sort

Bucket sort is mainly useful when input is uniformly distributed over a range. For example, consider the following problem.   

*Sort a large set of floating point numbers which are in range from 0.0 to 1.0 and are uniformly distributed across the range. How do we sort the numbers efficiently?*

A simple way is to apply a comparison based sorting algorithm. The [lower bound for Comparison based sorting algorithm](https://www.geeksforgeeks.org/lower-bound-on-comparison-based-sorting-algorithms/) (Merge Sort, Heap Sort, Quick-Sort .. etc) is Ω(n Log n), i.e., they cannot do better than nLogn.

Can we sort the array in linear time? [Counting sort](https://www.geeksforgeeks.org/counting-sort/) can not be applied here as we use keys as index in counting sort. Here keys are floating point numbers.

The idea is to use bucket sort. Following is bucket algorithm.

    bucketSort(arr[], n)
    1) Create n empty buckets (Or lists).
    2) Do following for every array element arr[i].
    .......a) Insert arr[i] into bucket[n*array[i]]
    3) Sort individual buckets using insertion sort.
    4) Concatenate all sorted buckets.

![](https://media.geeksforgeeks.org/wp-content/uploads/BucketSort.png)

**Time Complexity:** If we assume that insertion in a bucket takes O(1) time then steps 1 and 2 of the above algorithm clearly take O(n) time. The O(1) is easily possible if we use a linked list to represent a bucket (In the following code, C++ vector is used for simplicity). Step 4 also takes O(n) time as there will be n items in all buckets.   

The main step to analyze is step 3. This step also takes O(n) time on average if all numbers are uniformly distributed (please refer [CLRS book](http://www.flipkart.com/introduction-algorithms-3rd/p/itmdvd93bzvrnc7b?pid=9788120340077&affid=sandeepgfg) for more details)  

Following is the implementation of the above algorithm.

```cpp
// C++ program to sort an
// array using bucket sort
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

// Function to sort arr[] of
// size n using bucket sort
void bucketSort(float arr[], int n)
{

    // 1) Create n empty buckets
    vector<float> b[n];

    // 2) Put array elements
    // in different buckets
    for (int i = 0; i < n; i++) {
        int bi = n * arr[i]; // Index in bucket
        b[bi].push_back(arr[i]);
    }

    // 3) Sort individual buckets
    for (int i = 0; i < n; i++)
        sort(b[i].begin(), b[i].end());

    // 4) Concatenate all buckets into arr[]
    int index = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < b[i].size(); j++)
            arr[index++] = b[i][j];
}

/* Driver program to test above function */
int main()
{
    float arr[]
        = { 0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434 };
    int n = sizeof(arr) / sizeof(arr[0]);
    bucketSort(arr, n);

    cout << "Sorted array is \n";
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    return 0;
}
```

**Output**

    Sorted array is 
    0.1234 0.3434 0.565 0.656 0.665 0.897 

#### **Bucket Sort for numbers having integer part:**

**Algorithm** :

1. Find maximum element and minimum of the array

2. Calculate the range of each bucket

3. Create n buckets of calculated range

4. Scatter the array elements to these buckets
   
    BucketIndex = ( arr[i] - min ) / range

5. Now sort each bucket individually

6. Gather the sorted elements from buckets to original array
   
    Input :    
    Unsorted array:  [ 9.8 , 0.6 , 10.1 , 1.9 , 3.07 , 3.04 , 5.0 , 8.0 , 4.8 , 7.68 ]
    No of buckets :  5
   
    Output :  
    Sorted array:   [ 0.6 , 1.9 , 3.04 , 3.07 , 4.8 , 5.0 , 7.68 , 8.0 , 9.8 , 10.1 ]

![](https://media.geeksforgeeks.org/wp-content/uploads/20201210213123/bucketSort.png)

    Input :    
    Unsorted array:  [0.49 , 5.9 , 3.4 , 1.11 , 4.5 , 6.6 , 2.0]
    No of buckets: 3
    
    Output :  
    Sorted array:   [0.49 , 1.11 , 2.0 , 3.4 , 4.5 , 5.9 , 6.6]
