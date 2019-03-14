# In class definition：static
- Inside a class definition, the keyword [`static`](https://en.cppreference.com/w/cpp/keywords/static) [declares] members that are not bound to class instances.

- Outside a class definition, it has a different meaning: see [storage duration](https://en.cppreference.com/w/cpp/language/storage_duration).



## Syntax [ declaration ]

```CPP
static data_member	(1)	//脱离了对象这个概念，比如利率。
static member_function	(2)	//没有this指针，只能处理static 数据。
```


## Explanation
- Static members of a class are [not associated] with the [objects of the class]: they are independent variables with static [or thread (since C++11) ]storage duration or regular functions.

- The static keyword is only used with the [declaration] of a static member, inside the class definition,
- Not with the [definition] of that static member:
```CPP
class X { static int n; }; // declaration (uses 'static')
int X::n = 1;              // definition (does not use 'static')
```
- The declaration inside the class body is not a definition and may declare the member to be of incomplete type (other than void), including the type in which the member is declared:
```CPP
struct Foo;
struct S
{
   static int a[]; // declaration, incomplete type
   static Foo x;   // declaration, incomplete type
   static S s;     // declaration, incomplete type (inside its own definition)
};
 
int S::a[10]; // definition, complete type
struct Foo {};
Foo S::x;     // definition, complete type
S S::s;       // definition, complete type
```
- However, if the [declaration] uses constexpr [or inline (since C++17)] specifier, the member must be declared to have complete type.(since C++11)

  

## To refer to a static member m of class T

- 通过类名调用。T::m
- 通过静态对象调用。[E.m] or [E->m], where E is an expression that evaluates to [T] or [T*] respectively. 

```cpp
class Account
{
    public:
    static double m_rate;//declaration 静态数据变量
    static void set_rate(const double& x)//declaration 静态成员函数
    {
        m_rate = x;
    }
};

 
double Account::m_rate = 8.0;// definition

int main()
{
    Account::set_rate(5.0);//通过类名调用
    Account a;
    a.set_rate(7.0);//通过对象调用
   
}
```

----

Static members obey the [class member access rules (private, protected, public)](https://en.cppreference.com/w/cpp/language/access).

## Static member functions

- Static member functions are not associated with any object. When called, they have [no this pointer].

- Static member functions[ cannot be virtual, const, or volatile].

- The address of a static member function may be stored in a regular [pointer to function](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_functions), but not in a [pointer to member function](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_member_functions).

non-static member functions：有一个不可以写出来的隐藏参数指针this，代表被调用对象的地址。

```cpp
class complex
{
    public:
    double real() const{return this->re;}//被调用对象的地址
    private:
    double re,im;
}

complex c1,c2,c3;
cout<<c1.real();
cout<<complex::real(&c1);//this指针
```



## Static data members

- Static data members are not associated with any object. They exist even if **no objects of the class have been defined**. 
- There is only one instance of the static data member in the entire program with static [storage duration](https://en.cppreference.com/w/cpp/language/storage_duration), unless the keyword [`thread_local`](https://en.cppreference.com/w/cpp/keyword/thread_local) is used, in which case there is one such object per thread with thread storage duration (since C++11).

- Static data members **cannot be mutable**.(mutable: permits modification of the **class member declared mutable** even if the **containing object is declared const**.)

- Static data members of a class in namespace scope have [external linkage](https://en.cppreference.com/w/cpp/language/storage_duration) if the class itself has external linkage (i.e. is not a member of [unnamed namespace](https://en.cppreference.com/w/cpp/language/namespace#Unnamed_namespaces)). 

- Local classes (classes defined inside functions) and unnamed classes, including member classes of unnamed classes, cannot have static data members.

 

## Constant static members

If a static data member of integral or enumeration type is declared const (and not volatile), it can be initialized with an [initializer](https://en.cppreference.com/w/cpp/language/initialization) in which every expression is a [constant expression](https://en.cppreference.com/w/cpp/language/constexpr), right inside the class definition:

```cpp
struct X
{
    const static int n = 1;
    const static int m{2}; // since C++11
    const static int k;
};
const int X::k = 3;
```

### c++ 11

If a static data member of [*LiteralType*](https://en.cppreference.com/w/cpp/named_req/LiteralType) is declared constexpr, it must be initialized with an initializer in which every expression is a constant expression, right inside the class definition:

```cpp
struct X {
    constexpr static int arr[] = { 1, 2, 3 };        // OK
    constexpr static std::complex<double> n = {1,2}; // OK
    constexpr static int k; // Error: constexpr static requires an initializer
};
```



If a const non-inline (since C++17) static data member or a constexpr static data member (since C++11) is [odr-used](https://en.cppreference.com/w/cpp/language/definition#ODR-use), a definition at namespace scope is still required, but it cannot have an initializer. This definition is deprecated for`constexpr` data members (since C++17).

```cpp
struct X {
    static const int n = 1;
    static constexpr int m = 4;
};
const int *p = &X::n, *q = &X::m; // X::n and X::m are odr-used
const int X::n;             // … so a definition is necessary
constexpr int X::m;         // … (except for X::m in C++17)
```