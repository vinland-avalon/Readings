# Quick Sort
It's an amazing *Divide and Conquer Algorithm* and Here I'd like to write something interesting about it.
## Reference
- [QuickSort](https://www.geeksforgeeks.org/quick-sort/)  
- [3-way QuickSort (Dutch National Flag)](https://www.geeksforgeeks.org/3-way-quicksort-dutch-national-flag/)
- [QuickSort on Singly Linked List]
- [QuickSort on Doubly Linked List]
## The Easiest Way to Implement a Basic QuickSort Algorithm
```C++
void quickSort(vector<int>& nums) {
    helper(nums, 0, nums.size() - 1);
}
void helper(vector<int>& nums, int left, int right) {
    if(left >= right) return;
    int index = partition(nums, left, right);
    helper(nums, left, index - 1);
    helper(nums, index + 1, right);
}
// [left, ..., index - 1] less or equal to nums[index]
// [index + 1, ..., right] greater or equal to nums[index]
int partition(vector<int>& nums, left, right) {
    int p = left;
    while(left < right) {
        while(left < right && nums[right] >= nums[p]) right--;
        while(left < right && nums[left] <= nums[p]) left++;
        swap(nums[left], nums[right]);
    }
    swap(nums[left], nums[p]);
    return left;
}
```
The abovementioned algorithm takes the first element in range as *pivot*, however, to get a more stable sorting, it could be randomly chosen.
```C++
private:
    mt19973 gen{random_device{}()};
    // ...
    // skip some code and directly comes into function partition
    // ...
    int p = uniform_int_distribution<int>{left, right}(gen);
```
## 3-way QuickSort
It helps to deal with array within which exist lots of redundant elements. It divides the target range into 3 parts, *less, equal and greater*.  
It can be used in [143 · Sort Colors II](https://www.lintcode.com/problem/143/)
```python
In 3 Way QuickSort, an array arr[l..r] is divided in 3 parts:
a) arr[l..i) elements less than pivot.
b) arr[i..j) elements equal to pivot.
c) arr[j..r] elements greater than pivot.
```
```C++
void helper(vector<int>& nums, int left, int right) {
    if (left < right) return;
    int v = nums[left];
    // in this phrase, we will not sort nums[left], 
    // so i is not equal to left-1 and j is not equal to left
    i = left;
    j = right + 1;
    int curr = i + 1;
    // keep nums[i] < v for each iteration, and it will exchange after loop
    while (curr < j) {
        if (nums[curr] < v) {
            swap(nums[i+1], nums[curr]);
            curr++;
            i++;
        } else if (nums[curr] > v) {
            // curr wont change, since it should be judged again
            swap(nums[j - 1], curr);
            j--;
        } else {
            curr++;
        }
    }
    swap(nums[left], nums[i]);

    sort(nums, left, i - 1);
    sort(nums, j, right);
}

```
