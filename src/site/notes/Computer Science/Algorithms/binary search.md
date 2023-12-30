---
{"dateCreated":"2022-05-06 00:51","tags":["data_structures","algorithms"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/algorithms/binary-search/","dgPassFrontmatter":true}
---


# binary search
> **Binary Search** is a [searching algorithm](https://www.geeksforgeeks.org/searching-algorithms/) used in a sorted [[Computer Science/Data Structures/array\|array]] by **repeatedly dividing the search interval in half**. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to O(Log n). 

**Binary Search Algorithm:** The basic steps to perform Binary Search are:

-   Begin with an interval covering the whole array.
-   If the value of the search key is less than the item in the middle of the interval, narrow the interval to the lower half.
-   Otherwise, narrow it to the upper half.
-   Repeatedly check until the value is found or the interval is empty.  
     

**Illustration of Binary Search Algorithm:** 

![](https://media.geeksforgeeks.org/wp-content/uploads/20220309171621/BinarySearch.png)

**Step-by-step Binary Search Algorithm:** We basically ignore half of the elements just after one comparison.

1.  Compare x with the middle element.
2.  If x matches with the middle element, we return the mid index.
3.  Else If x is greater than the mid element, then x can only lie in the right half subarray after the mid element. So we recur for the right half.
4.  Else (x is smaller) recur for the left half.

``` java

class BinarySearch {
// Returns index of x if it is present in arr[l.. r], else return -1
	int binarySearch(int arr[], int l, int r, int x)
	{
		if (r >= l) {
			int mid = l + (r - l) / 2;

			// If the element is present at the
			// middle itself
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

		// We reach here when element is not present
		// in array
		return -1;
	}

}
```

should mention that we can run a concurrent task that perform the trivial search and binary search and the first one to find the needed element will stop both tasks. 

the time complexity will be : $O(min \ [k,logn])$ 

### exponential search 
we can even make the search more efficient if we  perform the following steps: 
* we will move in the array in $2^i$ jumps until the first element that will be bigger than our needed element. 
* we will take the index $i$ that will ensure that $2^i\geq{k}$ 
and build an array where $2^i$ is the end of the array 
* perform binary search on the above array.


__Time complexity:__
$$2^{i-1}<k<2^i$$
therefore :
$$2^i<2k<2^{i+1}$$
and now : 
$$i = log{\ 2^i}<log(2k)=\log{k}+\log{2}=\log{k}+1\in{O(\log{k})}$$
so , we find an even better way to search in a sorted array, as needed. 

