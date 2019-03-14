# range-for[c++11]
> What is the correct way of using C++11's range-based for? 
>What syntax should be used? for (auto elem : container), or for (auto& elem : container) or for (const auto& elem : container)? Or some other?

>答案直接在Generic code 部分。
>下面是解释：

There're some difference between **observing the elements in the continer** vs. **modifying them in place**.

## Observing
### 1. cheap-to-copy :by value
适用简单类型的元素，比如```int```,```double```
```cpp
for (auto elem : container) 
```
#### eg1 ```for (auto x : v)```
observing "int" in "vector".
```cpp
vector<int> v = {1, 3, 5, 7, 9};

for (auto x : v)
    cout << x << ' ';
```
output:
```cpp
1 3 5 7 9
```

### 2.general case : by const reference
#### eg2 ```for (auto x : v)```
复杂类型的by value(效率低，会调用拷贝构造。)

observing "customclass_type" in "vector".
```cpp
// A sample test class, with custom copy semantics.
class X
{
public:
    X() 
        : m_data(0) 
    {}

    X(int data)
        : m_data(data)
    {}

    ~X() 
    {}

    X(const X& other) 
        : m_data(other.m_data)
    { cout << "X copy ctor.\n"; }

    X& operator=(const X& other)
    {
        m_data = other.m_data;       
        cout << "X copy assign.\n";
        return *this;
    }

    int Get() const
    {
        return m_data;
    }

private:
    int m_data;
};

ostream& operator<<(ostream& os, const X& x)
{
    os << x.Get();
    return os;
}

vector<X> v = {1, 3, 5, 7, 9};

cout << "\nElements:\n";
for (auto x : v)
{
    cout << x << ' ';
}
```
output: 在```vector<X>```初始化时，```拷贝构造函数```被调用了。
```cpp
Elements:
X copy ctor.
1 X copy ctor.
3 X copy ctor.
5 X copy ctor.
7 X copy ctor.
9
```
在range-based for loop iterations的时候，采用的是by value的方法，初始化容器中每个元素时，**copy constructor** 被调用了。这样做很低效，因为我们只想观察容器中的元素,而不是想拷贝一个临时对象。（e.g. if these elements are instances of std::string, heap memory allocations can be done, with expensive trips to the memory manager, etc.）



#### eg3 ```for (const auto& x : v)```
所以最好的观测方法是常量引用传递```const auto &```
```cpp
vector<X> v = {1, 3, 5, 7, 9};

cout << "\nElements:\n";
for (const auto& x : v)
{ 
    cout << x << ' ';
}
```
output:没有```拷贝构造函数```调用。更加高效。
```cpp
Elements:
1 3 5 7 9
```

## Modifying
如果要修改容器中的元素， ```for (auto elem : container)``` 或者```for (const auto& elem : container)``` 的写法是错误的。
#### eg4 ```for (auto elem : container)``` 
修改的是拷贝局部临时变量的值，不是原始数据
```cpp
vector<int> v = {1, 3, 5, 7, 9};
for (auto x : v)  // <-- capture by value (copy)
    x *= 10;      // <-- a local temporary copy ("x") is modified,
                  //     *not* the original vector element.

for (auto x : v)
    cout << x << ' ';
```
output
```cpp
1 3 5 7 9
```
#### eg5 ```for (const auto& elem : container)```
修改常量引用会导致编译不通过
```cpp
TestRangeFor.cpp:138:11: error: assignment of read-only reference 'x'
          x *= 10;
            ^
```

#### eg6 ```for (auto& x : v)```
正确方法是非常量引用
```cpp
vector<string> v = {"Bob", "Jeff", "Connie"};

// Modify elements in place: use "auto &"
for (auto& x : v)
    x = "Hi " + x + "!";

// Output elements (*observing* --> use "const auto&")
for (const auto& x : v)
    cout << x << ' ';
```
output
```cpp
Hi Bob! Hi Jeff! Hi Connie!
```

## Case of  **Proxy Iterator**
需求：想通过非常量引用修改容器中bool的状态。
#### eg7 ```for (auto& x : v)```
```cpp
vector<bool> v = {true, false, false, true};
for (auto& x : v)
    x = !x;
```
编译不通过
```cpp
TestRangeFor.cpp:168:20: error: invalid initialization of non-const reference of
 type 'std::_Bit_reference&' from an rvalue of type 'std::_Bit_iterator::referen
ce {aka std::_Bit_reference}'
     for (auto& x : v)
                    ^
```
为```bool```特化的```std::vector```模板，是由把```bool```打包优化空间后实现的，每个```boolean```值占1bit,8个```boolean```才占1byte。
因为不能返回一个单独的bit的reference，所以```vector<bool>```使用了一个```proxy iterator```设计模式。
```proxy iterator```在解引用的时候，不产生普通的```bool&```，而是返回(by value)一个临时对象，这个对象是一个可以表达```bool```的```proxy class```。


REF:
>The problem is that std::vector template is specialized for bool, with an implementation that packs the bools to optimize space (each boolean value is stored in one bit, eight "boolean" bits in a byte).

>Because of that (since it's not possible to return a reference to a single bit), vector<bool> uses a so called "proxy iterator" pattern. A "proxy iterator" is an iterator that, when dereferenced, does not yield an ordinary bool &, but instead returns (by value) a temporary object, which is a [proxy class convertible to bool](https://en.wikipedia.org/wiki/Sequence_container_%28C%2B%2B%29#Specialization_for_bool). (See also this question and related answers here on [StackOverflow](https://stackoverflow.com/questions/8399417/why-vectorboolreference-doesnt-return-reference-to-bool).)

#### eg8 ```for (auto&& x : v)```
```cpp
vector<bool> v = {true, false, false, true};

// Invert boolean status
for (auto&& x : v)  // <-- note use of "auto&&" for proxy iterators
    x = !x;

// Print new element values
cout << boolalpha;        
for (const auto& x : v)// 遵从observing的const auto&
    cout << x << ' ';
```
output
```cpp
false true true false
```
- ```proxy iterators```也遵从observing的```for (const auto& elem : container)```
- ```for (auto&& elem : container)```也适用```ordinary (non-proxy) iterators```,比如```vector<int>```或者```vector<string>```。

## Summary
- If the objects are cheap to copy (like ints, doubles, etc.) OR NEED to **make a local copy**.
```cpp
for (auto elem : container)    // capture by value
```
- For observing the complex elements (not need to copy)
```cpp
for (const auto& elem : container)    // capture by const reference
```
- For modifying the elements in place 
```cpp
for (auto& elem : container)    // capture by (non-const) reference
```
- If the container uses "proxy iterators" (like std::vector\<bool>) 
```cpp
for (auto&& elem : container)    // capture by &&
```


### Generic code 

- For observing the elements 
```cpp
for (const auto& elem : container)    // capture by const reference
```
- For modifying the elements in place 
```cpp
for (auto&& elem : container)    // capture by &&
```

