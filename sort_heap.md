# Heapsort
## using priority_queue
### max-heap(default)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> pq(nums.begin(), nums.end());
        for (int i = 0; i < k - 1; i++) {
            pq.pop();//将堆顶元素删除k-1次。
        }
        return pq.top();
    }
};
```
### min-heap  
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int num : nums) {
            pq.push(num);
            if (pq.size() > k) {
                pq.pop();
            }
        }
        return pq.top();
    }
};
```
## using multiset(rbt)
### min-heap(default)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        multiset<int> mset;
        for (int num : nums) {
            mset.insert(num);
            if (mset.size() > k) {
                mset.erase(mset.begin());//multiset没有pop()，只能用erase(it);
            }
        }
        return *mset.begin();//multiset也没有top()，他是
    }
};
```
### max-heap
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        multiset<int, greater<int>> mset(nums.begin(), nums.end());
        for (int i = 0; i < k - 1; i++) {
            mset.erase(mset.begin());
        }
        return *mset.begin();
    }
};
```
## implements a max-heap
In the above we have presented heap solutions using STL. You may also implement your own heap if you are interested. I suggest you to read the Heapsort chapter of Introduction to Algorithms if you are not familiar with it. The following code implements a max-heap.
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        buildMaxHeap(nums);
        for (int i = 0; i < k - 1; i++) {
            swap(nums[0], nums[--heapSize]);
            maxHeapify(nums, 0);
        }
        return nums[0];
    }
private:
    int heapSize;
    
    int left(int i) {
        return (i << 1) + 1;
    }
    
    int right(int i) {
        return (i << 1) + 2;
    }
    
    void maxHeapify(vector<int>& nums, int i) {
        int largest = i, l = left(i), r = right(i);
        if (l < heapSize && nums[l] > nums[largest]) {
            largest = l;
        }
        if (r < heapSize && nums[r] > nums[largest]) {
            largest = r;
        }
        if (largest != i) {
            swap(nums[i], nums[largest]);
            maxHeapify(nums, largest);//递归
        }
    }
    
    void buildMaxHeap(vector<int>& nums) {
        heapSize = nums.size();
        for (int i = (heapSize >> 1) - 1; i >= 0; i--) {
            maxHeapify(nums, i);
        }
    }
};
```



@Lightmare

12/3/19



[最大堆（创建、删除、插入和堆排序）](https://wangwangok.github.io/2017/05/09/data_struct_max_heap/)

@Lightmare

13/3/19