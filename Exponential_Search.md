# Exponential Search
## Reference
[https://www.tutorialspoint.com/Exponential-Search](https://www.tutorialspoint.com/Exponential-Search)
## How does it works?
It is fit for searches in very-large-scale array.  
It trys to first figure out a range where the wanted data exists and then turn to binary search in this range. The `range` is increased exponentially.
### Pseudo Code
```python
Input: An sorted array, start and end location, and the search key
Output: location of the key (if found), otherwise wrong location.

Begin
   if (end – start) <= 0 then
      return invalid location
   i := 1
   while i < (end - start) do
      if array[i] < key then
         i := i * 2 //increase i as power of 2
      else
         terminate the loop
   done
   call binarySearch(array, i/2, i, key)
End
```
### C++ implementation
```C++
#include<iostream>
using namespace std;

int binarySearch(int array[], int start, int end, int key) {
   if(start <= end) {
      int mid = (start + (end - start) /2); //mid location of the list
      if(array[mid] == key)
         return mid;
      if(array[mid] > key)
         return binarySearch(array, start, mid-1, key);
         return binarySearch(array, mid+1, end, key);
   }
   return -1;
}

int exponentialSearch(int array[], int start, int end, int key){
   if((end - start) <= 0)
      return -1;
      int i = 1; // as 2^0 = 1
      while(i < (end - start)){
         if(array[i] < key)
            i *= 2; //i will increase as power of 2
         else
            break; //when array[i] corsses the key element
   }
   return binarySearch(array, i/2, i, key); //search item in the smaller range
}

int main() {
   int n, searchKey, loc;
   cout << "Enter number of items: ";
   cin >> n;
   int arr[n]; //create an array of size n
   cout << "Enter items: " << endl;
   for(int i = 0; i< n; i++) {
      cin >> arr[i];
   }
   cout << "Enter search key to search in the list: ";
   cin >> searchKey;
   if((loc = exponentialSearch(arr, 0, n, searchKey)) >= 0)
      cout << "Item found at location: " << loc << endl;
   else
      cout << "Item is not found in the list." << endl;
}
```