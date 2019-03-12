# priority_queue
Defined in header <queue>
```cpp
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```
- A **container adaptor** : O(1) for lookup of the largest element(by default) ; O(logn) for insertion and extraction.
- ```Compare```: change the ordering, e.g. using ```std::greater<T>``` makes the top the smallest.
- Similar to managing a heap in some **random access container**, with the benefit of not being able to accidentally invalidate the heap.
## Example
```cpp
#include <functional>
#include <queue>
#include <vector>
#include <iostream>
 
template<typename T> void print_queue(T& q) {
    while(!q.empty()) {
        std::cout << q.top() << " ";
        q.pop();
    }
    std::cout << '\n';
}
 
int main() {
    std::priority_queue<int> q; 
    //largest element(by default)
    for(int n : {1,8,5,6,3,4,0,9,7,2})
        q.push(n); 
    print_queue(q);//9 8 7 6 5 4 3 2 1 0 
 
    std::priority_queue<int, std::vector<int>, std::greater<int> > q2;
    //the top is the smallest.
    for(int n : {1,8,5,6,3,4,0,9,7,2})
        q2.push(n);
    print_queue(q2);//0 1 2 3 4 5 6 7 8 9
 
    // Using lambda to compare elements.
    auto cmp = [](int left, int right) { return (left ^ 1) < (right ^ 1);};
    std::priority_queue<int, std::vector<int>, decltype(cmp)> q3(cmp); 
    for(int n : {1,8,5,6,3,4,0,9,7,2})
        q3.push(n); 
    print_queue(q3);//8 9 6 7 4 5 2 3 0 1 
}
```



@Lightmare

19/3/12