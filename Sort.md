# Sort

## Inner Sort

所有数据已经读入内存，在内存中进行排序的算法。排序过程不需要对磁盘进行读写。辅助空间也直接存于内存之中。

### Comparison Sorts

- Insertion Sorts

	- Insertion Sort

	  每步将一个待排序的记录，按其关键码值的大小插入前面已经排序的文件中适当位置上，直到全部插入完为止。

		- 不适合对于数据量比较大的排序应用。但是，如果需要排序的数据量很小，例如，量级小于千，那么插入排序还是一个不错的选择。
		- 时间：Ω(n)。 Θ(n^2)。 O(n^2)。
		- 空间：O(1)。 

	- Shell Sort (improved InsertionSort)

		- 时间：Ω(nlogn)。 Θ(nlogn^2)。 O(nlogn^2)。
		- 空间：O(1)。 

- Exchange Sorts

	- Bubble Sort

		- 时间：Ω(n)。 Θ(n^2)。 O(n^2)。
		- 空间：O(1)。 

	- Quick Sort(Divide and  Conquer)

	  快排是平均状态下速度最快的一种排序算法。随机选定⼀个元素作为轴值，利⽤该轴值将数组分为左右两部分，左边元素都⽐轴值⼩，右边元素都⽐轴值⼤，但它们不是完全排序的。在此基础上，分别对左右两部分分别递归调⽤quick sort，使得左右部分完全排序。

		- 时间：Ω(nlogn) 。Θ(nlogn)  。O(n^2) 。
		- 空间：O(log(n)) 
		- 不稳定 


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

				int i = begin + 1;
				int j = end;

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

		## [REF: 对partition的优化](https://blog.csdn.net/ltyqljhwcm/article/details/53010800)

- Selection Sorts

	- Selection Sort

	  每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，再从剩余未排序元素中继续寻找最小（大）元素，放到已排序序列的末尾。

		- 选择排序是不稳定的排序方法。
		- 时间：Ω(n^2)。 Θ(n^2)。 O(n^2)。
		- 空间：O(1)。 

	- Heap Sort

		- Heap,将输入array变成最大/小值堆。
		- 堆的弹出操作：将堆顶元素与堆末元素交换，堆的⼤⼩减⼀，向下移动新的堆顶以维护堆的性质。相当于每次将剩余的最⼤元素移动到数组的最右边，重复这样的操作最终就能获得由⼩到⼤排序的数组。
		- 时间：Ω(nlogn) 。Θ(nlogn)。 O(nlogn)。

		  初次建堆的时间复杂度O(n)，删除堆顶元素并维护堆的性质需要O(logn)，这样的操作⼀共进⾏n次，故最终时间复杂度O(nlogn)

		- 空间：O(1)。 

	- Tree Sort

		- 时间：Ω(nlogn)。 Θ(nlogn)。 O(n^2)。 
		- 空间：O(n)。

- Timsort(mergeSort_insetionSort)
- Merge Sort(Divide and  Conquer)

	- 不依赖于随机读写，因此具有很强的普适性，适⽤于list。
	- 将线性数据结构（如array, vector或list ）分为两个部分，对两部分分别进⾏排序，排序完成后，再将各⾃排序好的两个部分合并还原成⼀个有序数组。
	- 时间：Ω(nlogn) 。Θ(nlogn)。 O(nlogn)。
	- 空间：O(n)。 

### Non-Comparison Sorts

- Bucket Sort 

	- 不需要数据之间的两两比较，但需要事先知道数组的一些具体情况。
	- 将数据根据其大小，放入合适的桶容器中，再依次从桶中取出，形成有序序列。
	- 桶排序适用于知道待排序数组大小范围的情况。
	- 时间：Ω(n+k)。 Θ(n+k)。 O(n^2)。
	- 空间：O(n)。

- Radix Sort

	- 不需要数据之间的两两比较，但需要事先知道数组的一些具体情况。
	- 时间：Ω(nk)。 Θ(nk)。 O(nk) 。
	- 空间：O(n+k)。

- Counting Sort

	- 时间：Ω(n+k)。 Θ(n+k)。 O(n+k) 。
	- 空间：O(k)。

## Outer Sort

内存中无法保存全部数据，需要磁盘访问，每次读入部分数据到内存进行排序。
外排序算法的核⼼思路在于把⽂件分块读到内存，在内存中对每块⽂件依次进⾏排序，最后合并排序后的各块数据，依次按顺序写回⽂件。相⽐于内排序，外排序需要进⾏多次磁盘读写，因此执⾏效率往往低于内排序，时间主要花费于磁盘读写上。

### Quick Selection

利用快排的思想，将数组依照⼀个轴值分割成两个部分，左边元素都⽐轴值⼩，右边元素都⽐轴值⼤。由于轴值下标已知，则可以判断所求元素落在数组的哪⼀部分，并在那⼀部分继续进⾏上述操作，直⾄找到该元素。

- 平均O(n)时间内从⼀个⽆序数组中返回第k⼤的元素。
- 与快排不同，由于快速选择算法只在乎所求元素所在的那⼀部分，故效率可以从O(nlogn)进⼀步提升⾄O(n)。

### Core Mind

- 假设⽂件需要分成k块读⼊，需要从⼩到⼤进⾏排序：
- 1) 依次读⼊每个⽂件块，在内存中对当前⽂件块进⾏排序(应⽤恰当的内排序算法)。此时，每块⽂件相当于⼀个由⼩到⼤排列的有序队列
- 2) 在内存中建⽴⼀个最⼩值堆，读⼊每块⽂件的队列头
- 3) 弹出堆顶元素，如果元素来⾃第i块，则从第i块⽂件中补充⼀个元素到最⼩值堆。弹出的元素暂存⾄临时数组
- 4) 当临时数组存满时，将数组写⾄磁盘，并清空数组内容。
- 5) 重复过程3)， 4)，直⾄所有⽂件块读取完毕

## Heap

### Method

- Add O(logN)
- Remove O(logN）
- Min/Max O(1)
- Build heap O(n)
- Heap sort O(nlogn)

### Implementation

- left child : i*2
- right chilf: i*2+1

### STL

- std::priority_queue

### Example

- Merge K Sorted List
- K-th Largest
- Closest to Origin

## Binary Sort

### Ordered linear container

### Recursion

- start + 1 < end
- left + (right - left) / 2
- A[mid] == , < , >
- A[start/end] == target

### Programming Pearls

- A generic binary search template

## Scalability & Memory Limits

### Divide & Conquer

### Hash function

### Hash table
