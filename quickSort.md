# Quicksort
- Also known as (partition-exchange sort).

- Average : O(nlogn), Worst : O(n^2)，但一般O(n^2)不常见。

- Faster than other O(nlogn) algorithms，因为Quicksort的inner loop在大部分的架构上很有效率被实现。

  

## Algorithm
- [divide and conquer algorithm](https://en.wikipedia.org/wiki/Divide_and_conquer_algorithm): 

    把一个序列分为两个子序列，再递归地解决这些子问题，最后这些子问题地组合为原问题的解。
- The steps are:
    1. Pick a pivot
    2. Partitioning
    3. Recursively


## Partition
### 1. Easy-version(S:Ω(n)) 
#### Pseudocode:
```python
function quicksort(q)
     var list less, pivotList, greater
     if length(q) ≤ 1 {
         return q
     } else {
         select a pivot value pivot from q
         for each x in q except the pivot element
             if x < pivot then add x to less
             if x ≥ pivot then add x to greater
         add pivot to pivotList
         return concatenate(quicksort(less), pivotList, quicksort(greater))
     }
```
需要Ω(n)的额外存储空间，跟Mergesort一样不好。额外需要的存储器空间配置，在实际上的实现，也会极度影响速度和缓存的性能。
### 2. In-place-version(S:O(log n))
有一个比较复杂使用原地（in-place）分区算法的版本，且在好的基准选择上，平均可以达到O(log n)空间的使用复杂度。
#### Pseudocode:
```python
function partition(a, left, right, pivotIndex)
     pivotValue := a[pivotIndex]
     swap(a[pivotIndex], a[right]) // 把pivot移到結尾
     storeIndex := left
     for i from left to right-1
         if a[i] <＝ pivotValue
             swap(a[storeIndex], a[i])
             storeIndex := storeIndex + 1
     swap(a[right], a[storeIndex]) // 把pivot移到它最後的地方
     return storeIndex
```
这是原地分区算法，它分区了标示为”左边（left）”和”右边（right）”的序列部分，借由移动小于a[pivotIndex]的所有元素到子序列的开头，留下所有大于或等于的元素接在他们后面。在这个过程它也为基准元素找寻最后摆放的位置，也就是它回传的值。它暂时地把基准元素移到子序列的结尾，而不会被前述方式影响到。由于算法只使用交换，因此最后的数列与原先的数列拥有一样的元素。要注意的是，一个元素在到达它的最后位置前，可能会被交换很多次。 
一旦我们有了这个分区算法，要写快速排列本身就很容易：
#### Pseudocode:
```python
procedure quicksort(a, left, right)
     if right > left
         select a pivot value a[pivotIndex]
         pivotNewIndex := partition(a, left, right, pivotIndex)
         quicksort(a, left, pivotNewIndex-1)
         quicksort(a, pivotNewIndex+1, right)
``` 
## Implementation 
### 1. One-way scanning 
arr = [3,7,8,5,2,1,9,5,4]

从左到右（除了最后的基准元素），循环移动小于基准元素 5 的所有元素到数组开头，留下大于等于基准元素的元素接在后面。在这个过程它也为基准元素找寻最后摆放的位置。

![](https://includecmath.github.io/2017/01/08/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F/400px-Partition_example.svg.png) 

### In C++:
```cpp
int random(int min,int max)
{
    int random =rand()%(max-min+1) +min;
    return random;
}

void swap(int *in1,int* in2){
    int temp=*in1;
    *in1=*in2;
    *in2=temp;
}

int Partition(vector<int>& nums,int start,int end)
{
    if(start<0||end>=nums.size())
        return 0;

    int pivot = random(start,end);
    swap(&nums[pivot],&nums[end]);//把基准移到尾部
    
    int left = start -1;//最左边原地划分一个子集
    for(int i =start;i<end;i++)
    {
        if(nums[i]<nums[end])//如果小于基准就放入子集中
        {
            left++;
            if(left!=i)
                swap(&nums[i],&nums[left]);
        }
    }

    left++;
    swap(&nums[left],&nums[end]);//将最尾部的基准放在左子集的下一个位置

    return left;//返回基准
}

void QuickSort_Recur(vector<int>& nums, int low, int high)
{ 
    if(low<high)
    {
        int index = Partition(nums,low,high);//找到一个基准，并将所有小于基准的数放在基准左边。
        QuickSort_Recur(nums,low,index-1);
        QuickSort_Recur(nums,index+1,high);
    }
} 
```

### 2. Two-way scanning
var items = [4, 2, 6, 5, 3, 9];

![](https://includecmath.github.io/2017/01/08/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F/quicksort_partition1.png)

In the previous example, the array becomes [4, 2, 3, 5, 6, 9] after one partition and the index returned is 4 (the last spot of the left pointer). After that, the left side of the overall array (items 0 through 3) is partitioned, as in the following figure.

![](https://includecmath.github.io/2017/01/08/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F/quicksort_21.png)
### In C++:
```cpp 
#define Max(a, b) ( (a > b) ? a : b )
#define Min(a, b) ( (a < b) ? a : b )
#define RANDOM_INIT()	srand(time(NULL))
#define RANDOM(L, R)	(L + rand() % ((R) - (L) + 1)) // gen a random integer in [L, R]
/**
* swap 2-element, orignal value 
*/
template<typename T>
    static inline void swap(T &x, T &y)
    {
        T _t(x);
        x = y;
        y = _t;
    }
        
/**
* the quick-sort partition routine
*/
template<typename T>
    static int partition_(T list[],int begin, int end) {
        int pivot_idx = RANDOM(begin,end);
        T pivot = list[pivot_idx];
        swap(list[begin], list[pivot_idx]);

        int i = begin + 1;//low
        int j = end;//high

        while(i <= j) {
            while((i <= end) && (list[i] <= pivot))
                i++;
            while((j >= begin) && (list[j] > pivot))
                j--;
            if(i < j)
                swap(list[i],list[j]);
        }

        swap(list[begin],list[j]);
        return j; // final pivot position
    }

/**
* quick sort an array of range [begin, end]
*/
template<typename T>
    static void quicksort(T list[],int begin,int end) {
        if( begin < end) {
            int pivot_idx = partition_<T>(list, begin, end);
            quicksort(list, begin, pivot_idx-1);
            quicksort(list, pivot_idx+1, end);
        }
    } 
```

## Optimizations
- BST : 快速排序是二叉查找树（二叉搜索树）的一个空间最优化版本。不是循序地把数据项插入到一个明确的树中，而是由快速排序组织这些数据项到一个由递归调用所隐含的树中。这两个算法完全地产生相同的比较次数，但是顺序不同。对于排序算法的稳定性指标，原地分割版本的快速排序算法是不稳定的。其他变种是可以通过牺牲性能和空间来维护稳定性的。

- Heapsort : Worst:O(nlog n)的优势，堆排通常比快排慢，但是快排也有最差情况发生O(n^2)。如果事先知道可以用堆排，就直接使用堆排。堆排序也拥有重要的特点，仅使用固定额外的空间（堆排序是原地排序），而即使是最佳的快速排序变化版本也需要 O(log n)的空间。然而，堆排序需要有效率的随机存取才能变成可行。

- Mergesort : Worst:O(nlog n)的优势。不像快排或堆排，归排是一个稳定排序，且可以轻易地被采用在linked list和存储在慢速访问媒体上(像是磁盘存储或网络连接存储的非常巨大数列)。尽管快排可以被重新改写使用在链串列上，但是它通常会因为无法随机存取而导致差的基准选择。归排的主要缺点，是在最佳情况下需要O(n)额外的空间。



## Reference

1. [Quicksort From Wikipedia](https://en.wikipedia.org/wiki/Quicksort#Lomuto_partition_scheme)

2. [对partition的优化](https://blog.csdn.net/ltyqljhwcm/article/details/53010800)

3. [快速排序](https://includecmath.github.io/2017/01/08/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F/)