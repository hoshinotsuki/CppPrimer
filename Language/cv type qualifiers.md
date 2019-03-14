# cv (const and volatile) type qualifiers
- const - defines that the type is constant.
- volatile - defines that the type is volatile.

## const
```cpp
class complex
{
public:
  complex (double r = 0, double i = 0): re (r), im (i) { }
  complex& operator += (const complex&);
  complex& operator -= (const complex&);
  complex& operator *= (const complex&);
  complex& operator /= (const complex&);
  double real () const { return re; }
  double imag () const { return im; }
private:
  double re, im;

  friend complex& __doapl (complex *, const complex&);
  friend complex& __doami (complex *, const complex&);
  friend complex& __doaml (complex *, const complex&);
};
```

- 只有member fun后面可以用 const定义。成员变量后面不可以写const，写在前面。

- const obj 如果调用 non-const member fun会编译出错。

![img](https://img-blog.csdnimg.cn/20190226193028398.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

错误：如果定义时没有写成void print() const{}; 就会报错。因为str是const-obj，不能用non-const member fun调用。 ![img](https://img-blog.csdnimg.cn/20190226193355101.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 例子：STD里的操作符重载

来自 TEMPLATE CLASS basic_string的定义。string类的operator[]。

- 用户可能拿[]来改变字符串。如String str[5]="a";由于string的实现机制是共享模式，不可以直接改变内部内容。只能拷贝一份再改变。所以要做cow(copy on write)设计。
- 如果const String str[2];str是const obj，其内部不可以动。所以不必考虑cow。const obj只会调用const fun，所以const fun不必写cow。

```cpp
reference operator[](size_type _Off)
{
    // 下标可变序列
    // 必须考虑cow  
    return (this->_Myptr()[_Off]);
}

const_reference operator[](size_type _Off) const
{
	// 下标不可变序列
    // 不必考虑cow
    return (this->_Myptr()[_Off]);
}
```

- 可见const属于签名的一部分，这两个函数可以重载。 
- **C++规定：如果类中有成员函数的const fun 和nonconst fun同时存在，则const obj默认调用const fun，non-const obj默认调用non-const fun。**
- **C++规定：non-const fun（obj可变）可以调用const fun（obj不可变）。 反过来报错。**

----

### 例子：《cpp primer》15节 -基类的定义

```cpp
class Quote {
	/* 
	** istream&：必须是非常量，因为本操作就是向流读出数据，其状态会改变；
	** Quote& ：必须是非常量，因为本操作就是向此对象写入数据，此对象值会改变；
	*/  
	friend istream& operator>>(istream&, Quote&);
 
	
	/*
	**ostream& ：必须是非常量，因为本操作就是向流写入数据，其状态会改变；
	**const Quote& ：引用是因为避免赋值实参；常量是因为通常打印对象不会改变对象本身的值；
	*/
	friend ostream& operator<<(ostream&, const Quote&);  

public:

	/*这种传递中的代码使用以下变量来控制编译；
	**变量：IN_CLASS_INITS/DEFAULT_FCNS；对应C++作用：类初始状态设置/default(默认)；
	*/
#if defined(IN_CLASS_INITS) && defined(DEFAULT_FCNS)
	//若定义类初始值设置及默认值，则定义默认构造函数,不接受任何实参；
	Quote() = default;  
#else
	/*若未定义类初始值及默认值，则定义构造函数，函数体空；
	**构造函数初始列表为新创建的数据成员price初始化，对应初始化值为 0.0；
	*/
	Quote() : price(0.0) { }
#endif // !defined(IN_CLASS_INITS) && defined(DEFAULT_FCNS)


	//初始化构造函数
	Quote(const string &book,double sales_price):
		bookNo(book),price(sales_price){ }

	//虚析构函数，动态绑定
#ifdef DEFAULT_FCNS
	virtual ~Quote() = default;
#else
	virtual ~Quote() {	}
#endif // DEFAULT_FCNS

	//const fun。 因为bookNo被期望是const obj，所以只能被const fun调用，必须定义为const fun，否则报错。
	string isbn() const 
	{ return bookNo; }

	//虚函数。将在派生类中重写，根据书的数量，采取不同的折扣算法。
	//因为price（定价）也是const obj,不会被改。所以定义为const fun。
	virtual double net_price(size_t n) const 
	{ return n*price; }//虚函数返回动态分配的自身副本


private:
	const string bookNo;//书号，被期望是const obj

protected:

#ifndef IN_CLASS_INITS
	const double price = 0.0;
#else
	const double price;
#endif // !IN_CLASS_INITS
};
```

## mutable

May appear in the declaration of a [non-static [class members] of non-reference non-const] type:

```cpp
class X {
  mutable const int* p;         // OK
  mutable int* const q;         // ill-formed
};
```

Mutable is used to specify that the member does not affect the externally visible state of the class (as often used for [mutexes, memo caches, lazy evaluation, and access instrumentation]).

```cpp
class ThreadsafeCounter {
  mutable std::mutex m; // The "M&M rule": mutable and mutex go together
  int data = 0;
 public:
  int get() const {
    std::lock_guard<std::mutex> lk(m);
    return data;
  }
  void inc() {
    std::lock_guard<std::mutex> lk(m);
    ++data;
  }
};
```