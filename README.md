# 说明
用来保存自己学习cpp笔记的库。
尽量自己写笔记。
主要参考《cpp primer》、cppreference.com和博客。


# 目录

## 1. 语言

### 变量和基本类型
- [inline](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/Inline.md) | 19.3.7 | 
- [cv限定符](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/cv%20type%20qualifiers.md) | 19.3.14 | const , mutable
- [类外的static](https://github.com/hoshinotsuki/CppPrimer/blob/master/stack&heap.md) | 18.8.4 |
- [pointer to function](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/Pointer%20declaration.md)| 19.3.12 |Initialized & Implicit Conversion,  Dereferencing, the lvalue identifying the pointed-to function,overload

### 表达式
- [运算符优先级](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/Operator.md) | 19.3.9 | 

### 语句
- [[c++11]range-for中的遍历和修改](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/range-for.md) | 19.3.10 | keyword: proxy iterators, by value, const by reference |

### 面向对象程序设计

- [OOP](https://github.com/hoshinotsuki/CppPrimer/blob/master/OOP/OOP.md) | 19.2.25 |  
- [Static members](https://github.com/hoshinotsuki/CppPrimer/blob/master/OOP/static%20members.md)| 19.3.8 |
- [Union](https://github.com/hoshinotsuki/CppPrimer/blob/master/OOP/Union.md)| 19.3.13 |[largest data] member, implementation-defined, Member allocating, Conctors and Dectors of members, Member lifetime, Anonymous unions, Union-like classes
- [Struct](https://github.com/hoshinotsuki/CppPrimer/blob/master/OOP/Struct.md)| 19.3.12 | 1.struct内每个成员相对struct首地址的offset，都是该成员大小的整数倍；2.struct变量的首地址是内部最大成员的倍数;3.sizeof(struct)为struct最宽基本类型成员大小的整数倍。

## 2. 头文件

## 3. 具名要求
- [Named requirements](https://github.com/hoshinotsuki/CppPrimer/blob/master/Language/Named%20requirements.md)| 19.3.12 |  keyword: FunctionObject, Predicate, BinaryPredicate, Compare |
## 4. 语言支持库

## 5. 通用工具库

## 6. 字符串库

## 7. 容器库

#### Container adaptors
- [std::priority_queue](https://github.com/hoshinotsuki/CppPrimer/blob/master/Container/priority_queue.md) | 19.3.12 |

## 8. 迭代器库

## 9. Algorithms library
#### Non-modifying sequence operations
#### Modifying sequence operations
#### Partitioning operations
#### Sorting operations
- [排序算法的总结](https://github.com/hoshinotsuki/CppPrimer/blob/master/Algorithm/Sort.md) | 19.3.8 | 
- [std::sort](https://github.com/hoshinotsuki/CppPrimer/blob/master/Algorithm/quickSort.md) | 19.3.11 | keyword: quicksort/in-place partition/one-way/two-way/O(n) 不需要额外空间  
- [std::nth_element](https://github.com/hoshinotsuki/CppPrimer/blob/master/Algorithm/nth_element.md) | 19.3.14 |
#### Binary search operations (on sorted ranges)
#### Other operations on sorted ranges
#### Set operations (on sorted ranges)
#### Heap operations
- [std::sort_heap](https://github.com/hoshinotsuki/CppPrimer/blob/master/Algorithm/sort_heap.md) | 19.3.12 | 用stl的优先队列和红黑树multiset实现。一个implements a max-heap的例子。
#### Minimum/maximum operations
#### Comparison operations
#### Permutation operations
#### Numeric operations
- iota[c++11]
#### Operations on uninitialized memory
#### C library [Defined in header <cstdlib>]
## 10. 数值库

## 11. IO库 
- [IO库](https://github.com/hoshinotsuki/CppPrimer/blob/master/IO.md) | 19.2.24 |

## 12. 本地化库

## 13. 正则表达式库

## 14. 原子操作库

## 15. 线程支持库

## 16. 文件系统库



# 刷题笔记
- [C/C++基础](https://github.com/hoshinotsuki/CppPrimer/blob/master/Exam/C%2B%2B%E5%9F%BA%E7%A1%80.md) | 19.3.13 |

- [操作系统](https://github.com/hoshinotsuki/CppPrimer/blob/master/Exam/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F.md) | 19.3.13 |

- [leetcode](https://github.com/hoshinotsuki/leetcodeSolution)  | 19.3.13 |

- [面试真题](https://github.com/hoshinotsuki/CppPrimer/blob/master/Exam/%E7%9C%9F%E9%A2%98.md)  | 19.3.9 |

