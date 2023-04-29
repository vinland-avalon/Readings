# Segment Tree
## Reference
- [Sum of given range](https://www.geeksforgeeks.org/segment-tree-sum-of-given-range/)
## What is Segment Tree Like?
It is quite good for range query.  
It is a full-binary tree.  
The internal nodes contain range as well as sum of range.
The internal nodes is non-overlapped.  
The leaf node contains real values of array.  
In fact, it looks like:
![segment-tree]()
## How to Do Range Query and Value Update?
### Definition
We have an array arr[0 . . . n-1]. We should be able to:  
- Find the sum of elements from index l to r where 0 <= l <= r <= n-1
- Change the value of a specified element of the array to a new value x. We need to do arr[i] = x where 0 <= i <= n-1.
### With Array
For range query, run a loop from l to r. To update, do arr[i]=x.
- time complexity: O(n) O(1)
- space complexity: O(n)
### With Prefix Tree
For range query, do sum[r]-sum[l]. To update, update all values after i.
- time complexity: O(1) O(n)
- space complexity: O(n), in fact O(2n)
### With Segment Tree
- time complexity: O(log(n)) O(log(n))
- space complexity: O(n), in fact O(2n-1)
## How does Segment Tree Work?
### Range Query (getSum)
```python
int getSum(node, l, r) {
    if the range of node is within the wanted l to r
        return value of node
    else if the range of node is completely outside the wanted l to r
        return 0
    else 
        return getSum(left child of node, l, r) + getSum(right child of node, l, r)
}
```
### Update Value
Calculate diff, and update the related nodes incursively.
## Two Implementations (in C++)
```C++
// only with range query
#include <bits/stdc++.h>

using namespace std;

class SegmentTree {
	vector<int> tree;
	int size;

public:
	SegmentTree(vector<int>& array) {
		size = array.size();
		tree.resize(4 * size);
		buildTree(array, 0, 0, size - 1);
	}

private:
	void buildTree(vector<int>& array, int treeIndex, int left, int right) {
		if (left == right) {
			tree[treeIndex] = array[left];
			return;
		}
		int mid = left + (right - left) / 2;
		buildTree(array, 2 * treeIndex + 1, left, mid);
		buildTree(array, 2 * treeIndex + 2, mid + 1, right);
		tree[treeIndex] = min(tree[2 * treeIndex + 1], tree[2 * treeIndex + 2]);
	}

	int query(int treeIndex, int left, int right, int queryLeft, int queryRight) {
		if (queryLeft <= left && right <= queryRight)
			return tree[treeIndex];
		int mid = left + (right - left) / 2;
		int minValue = INT_MAX;
		if (queryLeft <= mid)
			minValue = min(minValue, query(2 * treeIndex + 1, left, mid, queryLeft, queryRight));
		if (queryRight > mid)
			minValue = min(minValue, query(2 * treeIndex + 2, mid + 1, right, queryLeft, queryRight));
		return minValue;
	}

public:
	int query(int left, int right) {
		return query(0, 0, size - 1, left, right);
	}
};

int main() {
	vector<int> array = {1, 3, 2, 5, 4, 6};
	SegmentTree st(array);
	cout << st.query(1, 5) << endl; // 2
	return 0;
}

```
```C++
// C++ program to show segment tree operations like construction, query
// and update
#include <bits/stdc++.h>
using namespace std;

// A utility function to get the middle index from corner indexes.
int getMid(int s, int e) { return s + (e -s)/2; }

/* A recursive function to get the sum of values in the given range
	of the array. The following are parameters for this function.

	st --> Pointer to segment tree
	si --> Index of current node in the segment tree. Initially
			0 is passed as root is always at index 0
	ss & se --> Starting and ending indexes of the segment represented
				by current node, i.e., st[si]
	qs & qe --> Starting and ending indexes of query range */
int getSumUtil(int *st, int ss, int se, int qs, int qe, int si)
{
	// If segment of this node is a part of given range, then return
	// the sum of the segment
	if (qs <= ss && qe >= se)
		return st[si];

	// If segment of this node is outside the given range
	if (se < qs || ss > qe)
		return 0;

	// If a part of this segment overlaps with the given range
	int mid = getMid(ss, se);
	return getSumUtil(st, ss, mid, qs, qe, 2*si+1) +
		getSumUtil(st, mid+1, se, qs, qe, 2*si+2);
}

/* A recursive function to update the nodes which have the given
index in their range. The following are parameters
	st, si, ss and se are same as getSumUtil()
	i --> index of the element to be updated. This index is
			in the input array.
diff --> Value to be added to all nodes which have i in range */
void updateValueUtil(int *st, int ss, int se, int i, int diff, int si)
{
	// Base Case: If the input index lies outside the range of
	// this segment
	if (i < ss || i > se)
		return;

	// If the input index is in range of this node, then update
	// the value of the node and its children
	st[si] = st[si] + diff;
	if (se != ss)
	{
		int mid = getMid(ss, se);
		updateValueUtil(st, ss, mid, i, diff, 2*si + 1);
		updateValueUtil(st, mid+1, se, i, diff, 2*si + 2);
	}
}

// The function to update a value in input array and segment tree.
// It uses updateValueUtil() to update the value in segment tree
void updateValue(int arr[], int *st, int n, int i, int new_val)
{
	// Check for erroneous input index
	if (i < 0 || i > n-1)
	{
		cout<<"Invalid Input";
		return;
	}

	// Get the difference between new value and old value
	int diff = new_val - arr[i];

	// Update the value in array
	arr[i] = new_val;

	// Update the values of nodes in segment tree
	updateValueUtil(st, 0, n-1, i, diff, 0);
}

// Return sum of elements in range from index qs (query start)
// to qe (query end). It mainly uses getSumUtil()
int getSum(int *st, int n, int qs, int qe)
{
	// Check for erroneous input values
	if (qs < 0 || qe > n-1 || qs > qe)
	{
		cout<<"Invalid Input";
		return -1;
	}

	return getSumUtil(st, 0, n-1, qs, qe, 0);
}

// A recursive function that constructs Segment Tree for array[ss..se].
// si is index of current node in segment tree st
int constructSTUtil(int arr[], int ss, int se, int *st, int si)
{
	// If there is one element in array, store it in current node of
	// segment tree and return
	if (ss == se)
	{
		st[si] = arr[ss];
		return arr[ss];
	}

	// If there are more than one elements, then recur for left and
	// right subtrees and store the sum of values in this node
	int mid = getMid(ss, se);
	st[si] = constructSTUtil(arr, ss, mid, st, si*2+1) +
			constructSTUtil(arr, mid+1, se, st, si*2+2);
	return st[si];
}

/* Function to construct segment tree from given array. This function
allocates memory for segment tree and calls constructSTUtil() to
fill the allocated memory */
int *constructST(int arr[], int n)
{
	// Allocate memory for the segment tree

	//Height of segment tree
	int x = (int)(ceil(log2(n)));

	//Maximum size of segment tree
	int max_size = 2*(int)pow(2, x) - 1;

	// Allocate memory
	int *st = new int[max_size];

	// Fill the allocated memory st
	constructSTUtil(arr, 0, n-1, st, 0);

	// Return the constructed segment tree
	return st;
}

// Driver program to test above functions
int main()
{
	int arr[] = {1, 3, 5, 7, 9, 11};
	int n = sizeof(arr)/sizeof(arr[0]);

	// Build segment tree from given array
	int *st = constructST(arr, n);

	// Print sum of values in array from index 1 to 3
	cout<<"Sum of values in given range = "<<getSum(st, n, 1, 3)<<endl;

	// Update: set arr[1] = 10 and update corresponding
	// segment tree nodes
	updateValue(arr, st, n, 1, 10);

	// Find sum after the value is updated
	cout<<"Updated sum of values in given range = "
			<<getSum(st, n, 1, 3)<<endl;
	return 0;
}
//This code is contributed by rathbhupendra

```
