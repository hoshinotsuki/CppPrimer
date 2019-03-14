# Utilities library:Type support

- std::is_object
Defined in header <type_traits> (c++11)
    - **cv-qualified type**, value is true. 
    - **function, reference, or void types**, value is false.
```cpp
template< class T >
 struct is_object;
```
```cpp
#include <iostream>
#include <type_traits>
 
int main() {
    class cls {};
    std::cout << std::boolalpha;
    //IO library:IO manipulators 
    std::cout << std::is_object<int>::value << '\n';
    //true,cv-qualified type
    std::cout << std::is_object<int&>::value << '\n';
    //false,reference type
    std::cout << std::is_object<cls>::value << '\n';
    //true,cv-qualified type
    std::cout << std::is_object<cls&>::value << '\n';
    //false,reference type
}
```
# Named Requirements: Library-Wide

## 1 FunctionObject 
- A **FunctionObject** type is the type of an object that can be used on the **left of the function call operator**. （能以函数调用语法调用的对象。）

### Requirements 
The type T satisfies FunctionObject if
- The type T satisfies ```std::is_object```, and
Given
- f, a value of type T or const T
- args, suitable argument list, which may be empty

The following expressions must be valid:
|   Expression   |  Requirements    |
| ---- | ---- |
|   ```f(args)```   | performs a function call     |

### Notes
※ **Functions** and **references to functions** are not function object types, but can be used where function object types are expected due to  **function-to-pointer implicit conversion(pointers to functions指向函数的指针)**.

### Standard library
- All **pointers to functions** satisfy this requirement.

- All [Function objects] defined in <functional>
- Some [return types of functions] of <functional>

## 2 Predicate
- The Predicate requirements describe a callable that returns a value testable as a bool.谓词要求描述可调用对象返回可作为 bool 测试的值。 

- Predicate is typically used with algorithms that take input data (individual objects/containers) and a predicate, which is then called on input data to decide on further course of action. 
- Some examples of predicate usage in C++ standard library are:

    - ```std::all_of```, ```std::any_of```, ```std::none_of ``` 
    Take an array of elements and a predicate as an input. Call predicate on individual input elements, and return true if for all/any/none elements, predicate returns true.
    ```cpp
    int main()
    {
        std::vector<int> v(10, 2);
        std::partial_sum(v.begin(), v.end(), v.begin());
        //<numeric>,计算范围 [first, last) 的子范围中元素的部分和，并写入到始于 d_first 的范围。默认版本用 operator+ ，第二版本用给定的二元函数 op 对元素求和.
        std::cout << "Among the numbers: ";
        std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, " "));
        std::cout << '\n';
    
        //all_of:若一元谓词对范围中所有元素返回 true 则为 true ，否则为 false 。若范围为空则返回 true 。
        if (std::all_of(v.begin(), v.end(), [](int i){ return i % 2 == 0; })) {
            std::cout << "All numbers are even\n";
        }
        
        //none_of: 若一元谓词不对范围中任何元素返回 true 则为 true ，否则为 false 。若范围为空则返回 true 。
        if (std::none_of(v.cbegin(), v.cend(), std::bind(std::modulus<int>(),                  std::placeholders::_1, 2))) {
            std::cout << "None of them are odd\n";
        }
        
        //any_of: 若一元谓词对范围中至少一个元素返回 true 则为 true ，否则为 false 。若范围为空则返回 false 。
        struct DivisibleBy
        {
            const int d;
            DivisibleBy(int n) : d(n) {}
            bool operator()(int n) const { return n % d == 0; }
        };    
        if (std::any_of(v.cbegin(), v.cend(), DivisibleBy(7))) {
            std::cout << "At least one number is divisible by 7\n";
        }
    }
    ```
    output:
    ```cpp
    Among the numbers: 2 4 6 8 10 12 14 16 18 20 
    All numbers are even
    None of them are odd
    At least one number is divisible by 7
    ```
    - ```std::find_if``` 
    Take sequence of elements, and a predicate. 
    Return first element in the sequence, for which predicate returns value equal to true
    ```cpp
    int main()
    {
        int n1 = 3;
        int n2 = 5;
    
        std::vector<int> v{0, 1, 2, 3, 4};
    
        auto result1 = std::find(std::begin(v), std::end(v), n1);
        auto result2 = std::find(std::begin(v), std::end(v), n2);
    
        if (result1 != std::end(v)) {
            std::cout << "v contains: " << n1 << '\n';//v contains: 3
        } else {
            std::cout << "v does not contain: " << n1 << '\n';
        }
    
        if (result2 != std::end(v)) {
            std::cout << "v contains: " << n2 << '\n';
        } else {
            std::cout << "v does not contain: " << n2 << '\n';//v does not contain: 5
        }
    }
    ```

In other words, if an algorithm takes a [Predicate] pred and an [iterator] first, it should be able to test the object of the type pointed to by the iterator first using the given predicate via a construct like ```if(pred(*first)) {...}```.The function object [pred] shall not apply any [non-constant function] through the [dereferenced iterator]. This function object may be a [pointer to function] or [an object of a type with an appropriate function call operator]. 

## 3 BinaryPredicate
- BinaryPredicate is a set of requirements expected by some of the standard library facilities from the user-provided arguments.

- Given a BinaryPredicate bin_pred and a pair of iterators iter1 and iter2 or an iterator iter and a value value, the expression bin_pred(*iter1, *iter2) or, respectively, bin_pred(*iter, value), must be contextually convertible to bool.
In addition, evaluation of that expression is not allowed to call non-const member functions of the dereferenced iterators.
### Requirements
- Predicate
- CopyConstructible (unless otherwise specified)

## 4 Compare

- Compare is a set of requirements expected by some of the standard library facilities from the user-provided function object types.

- The return value of the function call operation applied to an object of a type satisfying Compare, when contextually converted to bool, yields true if the first argument of the call appears before the second in the strict weak ordering relation induced by this type, and false otherwise.

- As with any BinaryPredicate(二元谓词), evaluation of that expression is not allowed to call non-const functions through the dereferenced iterators.

### Requirements
The type T satisfies Compare if
- The type T satisfies BinaryPredicate, and
Given
- comp, an object of type T
- equiv(a, b), an expression equivalent to !comp(a, b) && !comp(b, a)

The following expressions must be valid and have their specified effects

![](C:\Users\Administrator\Desktop\捕获.JPG)

Note: comp induces a strict total ordering on the equivalence classes determined by equiv



@Lightmare

19/3/12