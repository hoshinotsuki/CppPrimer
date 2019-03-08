阅读effective c++ 04 （30页） 提到的static对象和堆与栈对象。“不同编译单元内定义的non-local static对象”。


# Stack与Heap / 内存管理

## 1.Stack

stack是在某个作用域内的一块内存空间。当调用函数时，函数本身会形成一个栈来保存函数的参数和返回地址。函数体内声明的所有变量，都来自stack的内存。

## 2.Heap / system heap

堆是操作系统提供的全局global内存空间。程序动态分配dynamic allocated时，从堆里面取一些区块blocks。


## 3.stack和heap的内存例子 / 类外的static声明

```cpp
class Complex { … };
...
{
Complex c1(1,2);//stack/non-static local
static Complex c2(1,2);//static local 
    
Complex* p = new Complex(3);
...
delete p;    
}
Complex c3(1,2);//global/static
```

- stack / 构造函数型：
  - 局部对象 c1 的内存来自stack，生命周期在scope结束时结束，所以又称auto object，因为会被自动清理。
- heap / new型 动态分配：
  - 动态分配的临时对象Complex(3) ，占用空间来自heap。指针p指向这个临时对象，p的生命在它被delete的时候结束。如果当作用域结束，p指的堆对象依然存在，但是p的生命结束了，作用域外再也没有p，无法delete p，会导致内存泄漏leak。 
- static 对象：程序结束，自动销毁
  - local static  object。 c2 。在函数第一次调用时构造初始化。在程序结束之后自动析构。
  - non-local static  object
    - global object。c3。main调用之前被构造初始化。在程序结束之后自动析构。
    - 定义于namespace作用域的对象。main调用之前被构造初始化。在程序结束之后自动析构。

# 类外的static声明 

## 1. 两个编译单元中的non-local static object相互引用

注意：在同一个文件或不同编译单元（不同文件）中，如果存在多个non-local static object，它们都是在主函数调用之前被构造的，但是它们之间的构造顺序时不定的。即对编译器来说，静态成员对象之间的初始化顺序和析构顺序是一个未定义的行为。

因此，不能用某个non-local static object去初始化non-local static object，无论这两个non-local static object在不在同一个编译单元中。 

```cpp
class FileSystem

{

public: …

std::size_t numDisks() const;

};

extern FileSystem tfs;

//另一编译单元

class Directory

{

public:

Directory(params);

};

Directory::Directory(params)

{

std::size_t disks = tfs.numDisks();//使用另一个编译单元的静态变量

}
Directory tempDir (params);
```

由于编译器没有定义non-local static object之间的构造顺序，所以有可能类tfs还没有被构造，所以程序可能会报错。

## 2.解决方法

用local static对象替换non-local static对象。

C++保证，函数内的local static 对象会在该函数被调用期间，首次遇上该对象定义式时被初始化。 

```cpp
class FileSystem {…};

FileSystem& tfs()

{

static FileSystem fs;

return fs;

}

//另一编译单元

class Directory {…};

Directory::Directory (params)

{

std::size_t disks = tfs().numDisks();//执行函数tfs时，对象fs肯定会被构造。

};

Directory& tempDir()

{

static Directory td;

return td;

}
```



# 关于new型的内存分配

## 1.自定义类class型

### 1.new：先分配 memory, 再調用 ctor

```cpp
Complex* pc  =  new Complex(1,2);
```

編譯器轉化為

```cpp
Complex *pc;
void* mem = operator new( sizeof(Complex) ); //分配內存
pc = static_cast<Complex*>(mem); //轉型
pc->Complex::Complex(1,2); //構造函數
```

![img](https://img-blog.csdn.net/20180804231033613)


### 2.delete：先調用 dtor, 再釋放 memory

```cpp
Complex* pc = new Complex(1,2);
...
delete pc;
```

編譯器轉化為

```cpp
Complex::~Complex(pc); // 析構函數
operator delete(pc); // 釋放內存。其內部調用 free(pc)
```

![img](https://img-blog.csdn.net/20180804231238616)

## 2.String*型

### 1.new：先分配 memory, 再調用 ctor

```cpp
String* ps = new String("Hello String("Hello");
```

 編譯器轉化為

```cpp
String* ps;
void* mem = operator new( sizeof(String) ); //分配內存
ps = static_cast<String*>(mem); //轉型
ps->String::String("Hello"); //構造函數
```

![img](https://img-blog.csdn.net/20180804231547749)


### 2.delete：先調用 dtor, 再釋放 memory

```cpp
String* ps = new String("Hello");
...
delete ps;
```

編譯器轉化為

```cpp
String::~String(ps); // 析構函數
operator delete(ps); // 釋放內存
```

![img](https://img-blog.csdn.net/20180804231655275)


## 3.动态分配 

### 1.動態分配所得的內存塊 (memory block), in VC 
![img](https://img-blog.csdn.net/20180804231736838)



### 2.動態分配所得的 array 
![img](https://img-blog.csdn.net/20180804231809639)


### 3.array new 一定要搭配 array delete

```cpp
String* p = new String[3];
...
delete[] p; //喚起3次dtor 
```

```cpp
String* p = new String[3];
...
delete p; //喚起1次dtor
```

![img](https://img-blog.csdn.net/20180804231924999)

