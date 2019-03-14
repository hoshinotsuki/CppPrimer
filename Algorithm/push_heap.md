# push_heap
## Syntax
| Defined in header `<algorithm>`                              |               |      |
| ------------------------------------------------------------ | ------------- | ---- |
|                                                              | (1)           |      |
| template< class RandomIt > <br />void push_heap( RandomIt first, RandomIt last ); | (until C++20) |      |
|                                                              | (2)           |      |
| template< class RandomIt, class Compare > <br />void push_heap( RandomIt first, RandomIt last,                 Compare comp ); | (until C++20) |      |
|                                                              |               |      |
- Inserts the element at the position `last-1` into the *max heap* defined by the range `[first, last-1)`. 
- The first version of the function uses operator< to compare the elements, the second uses the given comparison function `comp`.

## Complexity

At most *log(N)* comparisons where N=[std::distance](http://en.cppreference.com/w/cpp/iterator/distance)(first, last).

## Example

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main()
{
    std::vector<int> v { 3, 1, 4, 1, 5, 9 };

    std::make_heap(v.begin(), v.end());
     
    std::cout << "v: ";
    for (auto i : v) std::cout << i << ' ';
    std::cout << '\n';
     
    v.push_back(6);
     
    std::cout << "before push_heap: ";
    for (auto i : v) std::cout << i << ' ';
    std::cout << '\n';
     
    std::push_heap(v.begin(), v.end());
     
    std::cout << "after push_heap: ";
    for (auto i : v) std::cout << i << ' ';
    std::cout << '\n';
}
```

输出：

```cpp
v: 9 5 4 1 1 3 
before push_heap: 9 5 4 1 1 3 6 
after push_heap:  9 5 6 1 1 3 4
```

@Lightmare

3/14/19