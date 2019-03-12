# 结构体成员存储对齐规则

其实字节对齐的细节和具体编译器实现相关，但一般而言，满足三个准则:
1. struct内每个成员相对struct首地址的offset，都是该成员大小的整数倍。
2. struct变量的首地址是内部最大成员的倍数;
3. sizeof(struct)为struct最宽基本类型成员大小的整数倍。

## 准则 1
结构(struct)或联合(union)的数据成员，第一个数据成员放在offset为0的地方，每个数据成员存储的起始位置要从该成员(每个成员本身)大小的整数倍开始(比如int在32位机为４字节,则要从４的整数倍地址开始存储)。
```cpp
struct node
{
     char a;//1
     int b//4
};
sizeof(node)=8
```
a存放在0offset的位置。b的存储起始位置为1，为了满足对齐原则，int大小为4，b的存储位置应该为4的整数倍，所以要在a后补齐，所以b的存储起始位置为4。所以node大小为8。 

## 准则2 结构体作为成员
如果一个结构里有某些结构体成员,则结构体成员要从```其内部最大元素大小的整数倍```地址开始存储.(struct a里存有struct b,b里有char,int ,double等元素,那b应该从8的整数倍开始存储.)
```cpp
struct pa
{
   char a;//1
   node b;//b内部最大元素大小是4，所以b的起始位置要是4的整倍数.在a后面补位。
}
sizeof(pa)=12//4+8=12.
``` 
# 准则3 sizeof(struct)
结构体的总大小,也就是sizeof的结果,.必须是其内部最大成员的整数倍（结构体成员以最大成员为代表）.不足的要补齐.
```cpp
struct node
{
    double a;//8
    char b;//1
}
sizeof(node)=16//9->2*8=16
```
按1,2原则可计算出结果应为9;
但结构体大小应为```最大成员的整倍数```，因此结果应为16
如果编译器中提供了```#pragma pack(n)```，上述对其模式就不适用了，例如设定变量以n字节对齐方式，则上述成员类型对齐宽度（应当也包括收尾对齐）应该选择```成员类型宽度和n中较小者```；
 
# 例子
 Q: 
```CPP
为什么sizeof操作员返回的结构尺寸大于结构成员的总尺寸？
```
A:
```CPP
struct X
{
    short s; /* 2 bytes */
             /* 2 padding bytes 准则1。struct内每个成员相对struct首地址的offset，都是该成员大小的整数倍。*/
    int   i; /* 4 bytes */
    char  c; /* 1 byte */
             /* 3 padding bytes 准则3：sizeof(struct)为struct最宽基本类型成员大小的整数倍*/
};

struct Y
{
    int   i; /* 4 bytes */
    char  c; /* 1 byte */
             /* 1 padding byte  准则1。struct内每个成员相对struct首地址的offset，都是该成员大小的整数倍。*/
    short s; /* 2 bytes */
};

struct Z
{
    int   i; /* 4 bytes */
    short s; /* 2 bytes */
    char  c; /* 1 byte */
             /* 1 padding byte 准则3：sizeof(struct)为struct最宽基本类型成员大小的整数倍*/
};

const int sizeX = sizeof(struct X); /* = 12 */
const int sizeY = sizeof(struct Y); /* = 8 */
const int sizeZ = sizeof(struct Z); /* = 8 */

可以通过对齐对成员进行排序来最小化结构的大小（按基本类型中的大小排序）（Z如上例中的结构）。
编译器自动对齐的原因：为了提高程序的性能，数据结构（尤其是栈）应该尽可能地在自然边界上对齐。原因在于，为了访问未对齐的内存，处理器需要作两次内存访问；然而，对齐的内存访问仅需要一次访问。

重要说明：C和C ++标准都声明结构对齐是实现定义的。因此，每个编译器可能选择以不同方式对齐数据，从而导致不同且不兼容的数据布局。因此，在处理将由不同编译器使用的库时，了解编译器如何对齐数据非常重要。某些编译器具有命令行设置和/或特殊#pragma 语句来更改结构对齐设置。
```
