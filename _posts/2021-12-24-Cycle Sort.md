---

layout: post
title: Cycle Sort
date: 2021-12-25 21:02:00
author: "phjung1"
header-img: "#"
tags:

- Algorithms
- Searching and Sorting
- Cycle Sort

---

# Cycle Sort

Cycle sort is an in-place sorting Algorithm, [unstable sorting algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability), a comparison sort that is theoretically optimal in terms of the total number of writes to the original array.



- It is optimal in terms of number of memory writes. It [minimizes the number of memory writes](https://www.geeksforgeeks.org/which-sorting-algorithm-makes-minimum-number-of-writes/) to sort (Each value is either written zero times, if it’s already in its correct position, or written one time to its correct position.)

- It is based on the idea that array to be sorted can be divided into cycles. Cycles can be visualized as a graph. We have n nodes and an edge directed from node i to node j if the element at i-th index must be present at j-th index in the sorted array.   
  Cycle in arr[] = {2, 4, 5, 1, 3}



![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/cycle-sort.png)



- Cycle in arr[] = {4, 3, 2, 1}

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/cyclc-sort2.png)



-We one by one consider all cycles. We first consider the cycle that includes first element. We find correct position of first element, place it at its correct position, say j. We consider old value of arr[j] and find its correct position, we keep doing this till all elements of current cycle are placed at correct position, i.e., we don’t come back to cycle starting point.



**Explanation :**

         arr[] = {10, 5, 2, 3}
     index =  0   1   2   3
    cycle_start = 0 
    item = 10 = arr[0]
    
    Find position where we put the item  
    pos = cycle_start
    i=pos+1
    while(i<n)
    if (arr[i] < item)  
        pos++;
    
    We put 10 at arr[3] and change item to 
    old value of arr[3].
    arr[] = {10, 5, 2, 10} 
    item = 3 
    
    Again rotate rest cycle that start with index '0' 
    Find position where we put the item = 3 
    we swap item with element at arr[1] now 
    arr[] = {10, 3, 2, 10} 
    item = 5
    
    Again rotate rest cycle that start with index '0' and item = 5 
    we swap item with element at arr[2].
    arr[] = {10, 3, 5, 10 } 
    item = 2
    
    Again rotate rest cycle that start with index '0' and item = 2
    arr[] = {2, 3,  5, 10}  
    
    Above is one iteration for cycle_stat = 0.
    Repeat above steps for cycle_start = 1, 2, ..n-2



```cpp
// C++ program to implement cycle sort
#include <iostream>
using namespace std;

// Function sort the array using Cycle sort
void cycleSort(int arr[], int n)
{
	// count number of memory writes
	int writes = 0;

	// traverse array elements and put it to on
	// the right place
	for (int cycle_start = 0; cycle_start <= n - 2; cycle_start++) {
		// initialize item as starting point
		int item = arr[cycle_start];

		// Find position where we put the item. We basically
		// count all smaller elements on right side of item.
		int pos = cycle_start;
		for (int i = cycle_start + 1; i < n; i++)
			if (arr[i] < item)
				pos++;

		// If item is already in correct position
		if (pos == cycle_start)
			continue;

		// ignore all duplicate elements
		while (item == arr[pos])
			pos += 1;

		// put the item to it's right position
		if (pos != cycle_start) {
			swap(item, arr[pos]);
			writes++;
		}

		// Rotate rest of the cycle
		while (pos != cycle_start) {
			pos = cycle_start;

			// Find position where we put the element
			for (int i = cycle_start + 1; i < n; i++)
				if (arr[i] < item)
					pos += 1;

			// ignore all duplicate elements
			while (item == arr[pos])
				pos += 1;

			// put the item to it's right position
			if (item != arr[pos]) {
				swap(item, arr[pos]);
				writes++;
			}
		}
	}

	// Number of memory writes or swaps
	// cout << writes << endl ;
}

// Driver program to test above function
int main()
{
	int arr[] = { 1, 8, 3, 9, 10, 10, 2, 4 };
	int n = sizeof(arr) / sizeof(arr[0]);
	cycleSort(arr, n);

	cout << "After sort : " << endl;
	for (int i = 0; i < n; i++)
		cout << arr[i] << " ";
	return 0;
}



```



**Output:**

    After sort : 
    1 2 3 4 8 9 10 10 



**Time Complexity** : O(n2)   
Worst Case : O(n2)   
Average Case: O(n2)   
Best Case : O(n2)  
This sorting algorithm is best suited for situations where memory write or swap operations are costly. 

 
