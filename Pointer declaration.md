# pointer to function
## Initialized & Implicit Conversion
- A [pointer to function] can be initialized with an address of a [non-member function] or a [static member function].  
- [function-to-pointer implicit conversion]
```cpp 
void f(int);
void (*p1)(int) = &f;
void (*p2)(int) = f; // same as &f, [function-to-pointer implicit conversion]
```
## [this] invokes the [pointed-to function]
- Unlike [functions] or [references to functions], [pointers to functions] are [objects] and thus can be stored in [arrays, copied, assigned], etc.
- A [pointer to function] can be used as the [left-hand operand] of the function call operator, [this] invokes the [pointed-to function]:
```cpp
int f(int n)
{
    std::cout << n << '\n';
    return n * n;
}
 
int main()
{
    int (*p)(int) = f;//(int)->argument type
    int x = p(7);//p invokes f
}
```
## Dereferencing
Dereferencing a function pointer yields the [lvalue identifying the pointed-to function]:
```cpp
int f();
int (*p)() = f;  
// pointer p is pointing to f
int (&r)() = *p; 
// the [lvalue that identifies f] is bound to a [reference]
r();             
// function f invoked through [lvalue reference]
(*p)();          
// function f invoked through the [function lvalue]
p();             
// function f invoked directly through the [pointer]
```
## Initialized from an overload
A pointer to function may be initialized from an overload set which may include **functions, function template specializations, and function templates**, if only one overload matches the type of the pointer (see address of an overloaded function for more detail):
```cpp
template<typename T> T f(T n) { return n; }
double f(double n) { return n; }
 
int main()
{
    int (*p)(int) = f; // instantiates and selects f<int>
}
```
Equality comparison operators are defined for pointers to functions (they compare equal if pointing to the same function). 
