# Union
## Rules
1. The union is only as big as necessary to hold its [largest data] member. 

2. The other data members are allocated in the [same bytes] as part of that [largest member]. 

3. The details of that allocation are [implementation-defined], and it's [undefined behavior] to read from the member of the union that [wasn't most recently written]. 

4. Many compilers implement, as a [non-standard language extension], the ability to [read inactive] members of a union.

   

```cpp
#include <iostream>
#include <cstdint>
union S
{
    std::int32_t n;     
    // occupies 4 bytes
    
    std::uint16_t s[2]; 
    // uint16_t:unsigned integer type with width of exactly 16
    // occupies 4 bytes(2*2=4)
    
    std::uint8_t c;   
    // uint8_t:unsigned integer type with width of exactly 8
    // occupies 1 byte
};                      
// the whole union occupies 4 bytes[rule.1]
 
int main()
{
    S s = {0x12345678}; // initializes the first member, s.n is now the active member
    // at this point, reading from s.s or s.c is undefined behavior[rule.3]    
    std::cout << std::hex << "s.n = " << s.n << '\n'; 
    
    s.s[0] = 0x0011; // s.s is now the active member
    
    // at this point, reading from n or c is UB but most compilers define it[rule.4]
    std::cout << "s.c is now " << +s.c << '\n' // 11 or 00, depending on platform
              << "s.n is now " << s.n << '\n'; // 12340011 or 00115678
}
```
output
```cpp
s.n = 12345678
s.c is now 0
s.n is now 115678
```

## Member allocating
Each member is allocated as if it is the only member of the class.
## Conctors and Dectors of members
If members of a union are classes with [user-defined constructors] and [destructors], to switch the active member, [explicit destructor] and [placement new] are generally needed:
```cpp
#include <iostream>
#include <string>
#include <vector>
 
union S
{
    std::string str;
    std::vector<int> vec;
    ~S() {} // needs to know which member is active, only possible in union-like class 
};// the whole union occupies max(sizeof(string), sizeof(vector<int>))[rule.1]
 
int main()
{
    S s = {"Hello, world"};
    // at this point, reading from s.vec is undefined behavior[rule.3]
    std::cout << "s.str = " << s.str << '\n';
    s.str.~basic_string();
    
    new (&s.vec) std::vector<int>;//placement new
    // now, s.vec is the active member of the union
    s.vec.push_back(10);
    std::cout << s.vec.size() << '\n';
    s.vec.~vector();//explicit destructor
}
```
output
```cpp
s.str = Hello, world
1
```

## Member lifetime
```cpp
union A { int x; int y[4]; };
struct B { A a; };
union C { B b; int k; };
int f() {
  C c;               // does not start lifetime of any union member
  c.b.a.y[3] = 4;    // OK: "c.b.a.y[3]", names union members c.b and c.b.a.y;
                     // This creates objects to hold union members c.b and c.b.a.y
  return c.b.a.y[3]; // OK: c.b.a.y refers to newly created object
}
 
struct X { const int a; int b; };
union Y { X x; int k; };
void g() {
  Y y = { { 1, 2 } }; // OK, y.x is active union member (9.2)
  int n = y.x.a;
  y.k = 4;   // OK: ends lifetime of y.x, y.k is active member of union
  y.x.b = n; // undefined behavior: y.x.b modified outside its lifetime,
             // "y.x.b" names y.x, but X's default constructor is deleted,
             // so union member y.x's lifetime does not implicitly start
}
```

## Anonymous unions
```cpp
int main()
{
    union
    {
        int a;
        const char* p;
    };
    a = 1;
    p = "Jennifer";
}
```

## Union-like classes
A union-like class is either a (non-union) class that has at least one anonymous union as a member or a union. A union-like class has a set of variant members:
- the non-static data members of its member anonymous unions;

- in addition, if the union-like class is a union, its non-static data members that are not anonymous unions.

  

  Union-like classes can be used to implement tagged unions.
```cpp
#include <iostream>
 
// S has one non-static data member (tag), three enumerator members (CHAR, INT, DOUBLE), 
// and three variant members (c, i, d)
struct S
{
    enum{CHAR, INT, DOUBLE} tag;
    union
    {
        char c;
        int i;
        double d;
    };
};
 
void print_s(const S& s)
{
    switch(s.tag)
    {
        case S::CHAR: std::cout << s.c << '\n'; break;
        case S::INT: std::cout << s.i << '\n'; break;
        case S::DOUBLE: std::cout << s.d << '\n'; break;
    }
}
 
int main()
{
    S s = {S::CHAR, 'a'};
    print_s(s);
    s.tag = S::INT;
    s.i = 123;
    print_s(s);
}
```
output
```cpp
a
123
```



@Lightmare

3/13/19