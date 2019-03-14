# 0. 编译
Q:
```CPP
void func()
{
   char b[2]={0};
   strcpy(b,"aaaa");
}
以下说法那个正确() 
Debug版崩溃，Release版正常 
```
A:
```CPP
assert 含义是断言，它是标准C++的cassert头文件中定义的一个宏，用来判断一个条件表达式的值是否为ture,如果不为true, 程序会终止，并且报告出错误，这样就很容易将错误定位 
通常我们开发的程序有2种模式:Debug模式和Release模式
1. 在Debug模式下,编译器会记录很多调试信息,也可以加入很多测试代码,比如加入断言assert，方便我们程序员测试,以及出现bug时的分析解决。
2. Release模式下,就没有上述那些调试信息,而且编译器也会自动优化一些代码,这样生成的程序性能是最优的,但是如果出现问题,就不方便分析测试了。因为这样会覆盖不属于自己的内存，这是搭上了程序崩溃的列车，即未定义行为，出现什么后果都有可能的，程序员的职责是保证不越界，而不是追问越界之后会发生什么。
```
Q:
```CPP
以下叙述正确的是（ ）。
可以把define和if定义为用户标识符。×
可以把define定义为用户标识符，但不能把if定义为用户标识符。√
可以把if定义为用户标识符，但不能把define定义为用户标识符。×
define和if都不能定义为用户标识符。×
```
A:![](https://uploadfiles.nowcoder.com/images/20171002/6206174_1506922935557_12F2FB5EC728457533D81B407BEBFA16)
```CPP
预定义标识符：define scanf printf include。
※ 预定义标识符可以做为用户标识符。
```
Q:
```CPP

```
A:
```CPP

```
Q:
```cpp
引用标准库，下面的说法哪些是正确的？
正确答案: B   你的答案: B (正确)
A.语句#include "stdio. h" 是正确的，而且程序编译的速度比#include<stdio. h>要快
B.语句#include <stdio. h>是正确的，而且程序编译的速度比#include"stdio. h"要快
C.语句#include "stdio. h"和#include <stdio. h>都是正确的，程序编译速度没有区别
D.语句#include "stdio. h"是错误的
```
A:
```cpp
通过<>，（库里）一步就找到了
通过“”， 由于先从当前目录中找（未找到），再到库中寻找（找到），比前者多了一步，所以花费时间比前者多
```


# 1. C++基础

## 内存管理
### stack
Q:如下C++程序，请问刚进入func函数时，参数在栈中的形式可能为 （左侧为地址，右侧为数据—）
```CPP
int i=0x22222222； 
char szTest[]=”aaaa”;  //a的ascii码为0x61 
func(I, szTest);    //函数原型为void func(int a,char *sz); 
```
A:
```CPP
0x0013FCF0	0x0013FCF8
0x0013FCF4	0x22222222
0x0013FCF8	0x61616161
1，对于x86，栈的存储：从高到低
2，函数调用入栈顺序是：从右到左
```
Q:
```CPP

```
A:
```CPP

```
## 表达式
### sizeof
Q:
```CPP

```
A:
```CPP

```
Q：
```cpp
win32系统里，下面几个sizeof的运行结果是（）
int intValue=1024;
char str[]="Tencent";
const char* ch=str;
sizeof(intValue)=__a___;//4
sizeof(str)=__b____;//8
sizeof(ch)=____c___;//4
```
A:
```CPP
1. 
cppp ex4_29:
int x[10];   int *p = x;
cout << sizeof(x) / sizeof(*x);//10/1=10数组所占内存大小/数组每个元素所占内存的大小=数组的容量。
因为cpp数组不内置求容量的方法，可以用这个方法求数组容量。
无论是整型数组还是字符数组，数组名作为右值的时候都代表数组首元素的首地址。
数组名不发生降级（还是数组）：sizeof（数组名），&数组名 
数组发生降级（变成指针）: 数组名作为参数传参，数组名参与运算

cout << sizeof(p) / sizeof(*p);//4/4=1 
指针的大小/指针所指内容类型int的大小


2. 
char str1[]="Tencent";
char str2[]={'T','e','n','c','e','n','t'};
注意区别：
sizeof(str1)=8;//8*sizeof(char)=8;//对于一个数组，返回这个数组占的总空间."Tencent"敏感词有T e n c e n t \0八个字符，所以str数组的长度是8.
sizeof(str2)=7; //没有结束符
strlen(str1)=7;//strlen不区分是数组还是指针，就读到\0为止返回长度。并且strlen是不把\0计入字符串的长度的。
sizeof（char）=1;//char类型占一个字节
sizeof（int）=4;//int类型占4个字节
sizeof(ch)=sizeof(char*)=4;//ch是一个指针，在 C/C++ 中一个指针占4个字节 

3.《cppp》p139：sizeof返回一个表达式或者类型名字所占的字节数。满足右结合律,且与*优先级一样，所以sizeof *p等价于sizeof(*p)。返回值是size_t类型。sizeof不会计算运算对象的值。即使解引用一个无效指针，依然是一个安全的行为，因为指针并没有真正被使用。
语法：①sizeof(type);类型大小。②sizeof expr;表达式大小。
className data,*p;
sizeof(className);//className类型的对象所占空间大小
sizeof data;//同sizeof(className)
sizeof *p;//同sizeof(className)
sizeof p;//指针大小,4

4.ex4_28:输出每种空间内置类型大小（32位）
bool            is 1bytes.
char            is 1bytes.
wchar_t         is 2bytes.
char16_t        is 2bytes.
char32_t        is 4bytes.
short           is 2bytes.
int             is 4bytes.
long            is 4bytes.
long long       is 8bytes.
float           is 4bytes.
double          is 8bytes.
long double     is 8bytes.
```


### 运算符优先级
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
以下程序的输出结果是（1）。
main()
{
    int a=4,b=5,c=0,d;
    d=!a&&!b||!c;
    printf("%d\n",d);
}
```
A:
```CPP
单目优先级最高。d=0&&0||1。逻辑表达式从左至右。答案为1.

[扩展]
1.ex4_30：sizeof优先级和解引用*一级，次于成员选择，单目>双目
sizeof x + y      // (sizeof x)+y . "sizeof" has higher precedence than binary`\+`.
sizeof p->mem[i]  // sizeof(p->mem[i])
sizeof a < b      // sizeof(a) < b
sizeof f()        // 如果F返回空，则无定义。否则返回返回指的类型所占大小。

2.[]高于*,[]从左往右，*从右往左
int *c[4];//16。4个（int*)类型的数组=16
int(*d)[4];//4。指向4个int型的数组的指针=4
```

## 转型
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
#include <stdio.h>
int main(void)
{
    unsigned int a = 1;
    signed int b = -3;
    int c;
    (a+b>0)?(c=1):(c=0);
    printf("%d",c);
    return 0;
}
正确答案: 1   你的答案: 0 (错误) 
```
A:
```CPP
unsigned int + int = unsigned int + [unsigned int]
int 会转换为 unsigned int 
```
## 字符串、向量、数组
### 字符串

Q:

```CPP

```

A:

```CPP

```



### 数组
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
以下 C 语言指令：
int a[5] = {1,3,5,7,9};
int *p = (int *)(&a+1);
printf(“%d,%d”,*(a+1) ， *(p-1));
运行结果是什么？
```
A:
```CPP
[解]3,9
```

Q:
```CPP
下列关于对象数组的描述中，( B  )是错误的。
A.对象数组的下标是从 0 开始的
B.对象数组的数组名是一个常量指针
C.对象数组的每个元素是同一个类的对象
D.对象数组只能赋初值，而不能被赋值。
```
A:
```CPP
B:数组名永远不可能是指针，这是两个类型，只不过函数传递时数组名会退化成指针。
D:数组只能初始化赋初值，可以对数组中的成员赋值，但不能对数组赋值。
```
#### 指针数组

Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
设有以下定义：
a[4][3]={1,2,3,4,5,6,7,8,9,10,11,12};
int (*prt)[3]=a, *p=a[0];
则以下能够正确表示数组元素a[1][2]的表达式是哪些？ 
A. *((*prt+1)[2])
B. *(*(p+5))
C. (*prt+1)+2
D. *(*(a+1)+2)。√
```

A:

```CPP
主要考察.下标和指针转化公式：*(a+n) = a[n]

拆分二维数组
int a[4][3] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 };
拆分成：
int b[3] = {1, 2, 3 };
int c[3] = {4, 5, 6 };
int d[3] = {7, 8, 9 };
int e[3] = {10, 11, 12 };
那么就有：
a[4] = { b, c, d, e };  
a是数组a中第一个元素b的地址 

int (*prt)[3]=a
就是
int b[3];
int *prt = &b;
即定义了一个指向“数组第一个元素的地址”的指针prt；
a表示的正是b的地址。所以，这里等价于：prt = a。

A：* (( * prt+1)[2])
*a 即a[0]，也就是b；
( b+1) 表示 元素2的地址，也就是a[0][1]的地址；
(b+1)[2] → *( (b+1) + 2 ) = *(b+3) = b[3]，越界了！其实就是c[0]，VS上验证过，输出也是

B：* ( * (p+5))
int *p = a[0]，相当于int *p = b，遇到p直接用b替换就行了！
* (p+5)等价于b[5]，也就是c[2]，元素6，前面还多个*，所以这个错的也很明显。

C：( * prt+1)+2
( * a+1)+2,*优先级高于+,(b+1) + 2 = b+3，是4的地址，也就是c[0]的地址；同样错误。

D:下标和指针转化公式：*(a+n) = a[n]，这个正反都可以使用，而且很好用。
```





## 声明

### const
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
What is output if you compile and execute the following c code?
void main()
{
    int i = 11;
    int const *p = &i;
    p++;
    printf("%d",*p);
}
```
A:
```CPP
[解]Garbage value。p指是常量，不能改变所指地址。

[扩展]
1.const用来定义不能改变的变量。所以①对象必须初始化。②一经创造后不能赋值。
const int i =get_size();//正确。运行时初始化。
const int j = 42;//正确。编译时初始化。
const int k;//错误。未初始化。
j=33;//错误。不能赋值。
const int h = j;//正确。拷贝初始化。拷贝一个对象并不会改变它。

2.reference to const：不用做修改绑定的对象。
const int c=1024;
const int &r1=c;//正确。常量引用指向常量对象。
r1 = 31; //错误。常量引用不能赋值。
int &r2 = c;//错误。非常量引用不能指向常量对象。
int n=1;
const int &r3=n;//正确。常量引用指向非常量对象。

3.C++规定引用的类型必须和引用对象一致。
但是两种例外。
①初始化【常量引用】时，允许用任何表达式作为初始值。
②允许【常量引用】绑定【非常量】的对象、字面值、表达式。
int i =42;
const int &r1 = i;//正确。对象
const int &r2 = 42;//正确。字面值
const int &r3 = r1 *2 ;//正确。表达式
int &r4 = r1*2;//错误。非法。常量引用不可以被绑定到非常量引用上。因为常量引用对象的值是不可以改变的。

4.当一个常量引用被绑定到另一个类型上发生了什么？
double dval =3.14;
const int &r1 = dval;//r1的操作是整型运算。而dval是双精度。
为了让r1绑定一个整数，编译器会把dval转为int的临时变量：
const int temp =dval;
const int &r1 =temp;

5.const和指针
方法是：从右往左阅读。
①C++允许【常量引用】绑定【非常量】
double dval = 3.14;
const double *p  = &dval;//正确。p所指对象是常量。不能通过p改变p指向对象的值。（指向常量的指针："自以为"指向了常量，不会改变所指对象的值。）
②const指针
int num = 0;
int *const p = &num;//p是常量。类型是指针。p一直指向num
```
### storage_duration
- static 静态或线程存储期和内部链接。内存中只存在一个。
- extern 静态或线程存储期和内部链接。外部变量，可以供所有源文件使用
- thread_local 线程存储期。
- mutable 不影响存储期或链接。
- register (c++17弃用) 放在寄存器中，而非内存中， 效率更高，一般是临时变量
- auto (c++11前) 默认都是自动变量 
```cpp
#include <iostream>
#include <string>
#include <thread>
#include <mutex>
 
thread_local unsigned int rage = 1; 
std::mutex cout_mutex;//a synchronization primitive that can be used to protect shared data from being simultaneously accessed by multiple threads.
 
void increase_rage(const std::string& thread_name)
{
    ++rage; // 在锁外修改 OK ；这是线程局域变量
    std::lock_guard<std::mutex> lock(cout_mutex);
    std::cout << "Rage counter for " << thread_name << ": " << rage << '\n';
}
 
int main()
{
    std::thread a(increase_rage, "a"), b(increase_rage, "b");
 
    {
        std::lock_guard<std::mutex> lock(cout_mutex);
        std::cout << "Rage counter for main: " << rage << '\n';
    }
 
    a.join();
    b.join();
}
```
output
```cpp
Rage counter for a: 2
Rage counter for main: 1
Rage counter for b: 2
```

### pointer
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
若有以下说明和定义，在必要的赋值之后，对fun函数的正确调用语句是（）。
int fun (int *c)  { … } 
void main() 
{ 
int (*a)(int*)=fun,"b ( ),w[10],c; 
… 
} 

正确答案: B   你的答案: D (错误)
A. a=a(w)；
B. (*a)(&c)；
C. b=*b(w);
D. fun(b);
```

A:

```CPP
A: a(w)调用正确，但将其返回值赋值给a是错误的，因为返回值是整型数，而a是一函数指针，所以答案A是错误的。
B: 因为a指向了函数fun，所以它等价于fun(&c)的调用，是正确的。
C: 因b是一个返回值为整型数指针的函数，*b(w)本身就是错误的。
D: b后面省掉了括号，因为b是函数，故也是错误的。
```





# 2. C++标准库

### IO库
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
下面的程序执行后，文件test.txt中的内容是（）. 
#include <stdio.h> 
void fun (char *fname,char *st) 
{ 
FTLE *myf;int i; 
myf=fopen (fname,"w"); 
for (i=0;i<strlen(st);i++) fputc (st[i],myf); 
fclose (myf); 
} 
void main() 
{ 
fun("test.txt","new world")；
fun("test.txt","hello,"); 
} 
输出：
hello,
```
A:
```CPP
打开文件采用的是w方式，该方法表示会[将原文件清除]，然后再重新写入。

fopen存在下面几种模式：
r	以只读方式打开文件，该文件必须存在。
r+	以读/写方式打开文件，该文件必须存在。
rb+	以读/写方式打开一个二进制文件，只允许读/写数据。
rt+	以读/写方式打开一个文本文件，允许读和写。

w	打开只写文件，若文件存在则长度清为 0，即该文件内容消失，若不存在则创建该文件。
w+	打开可读/写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。

a	以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即[文件原先的内容会被保留]（EOF 符保留）。
a+	以附加方式打开可读/写的文件。若文件不存在，则会建立该文件，如果文件存在，则写入的数据会被加到文件尾后，即文件原先的内容会被保留（原来的 EOF 符不保留）。

wb	以只写方式打开或新建一个二进制文件，只允许写数据。
wb+	以读/写方式打开或建立一个二进制文件，允许读和写。
wt+	以读/写方式打开或建立一个文本文件，允许读写。

at+	以读/写方式打开一个文本文件，允许读或在文本末追加数据。
ab+	以读/写方式打开一个二进制文件，允许读或在文件末追加数据。
```

Q:

```CPP
已知：int x，y；double z；以下语句中错误的函数调用是（D）。 
A.scanf(“%d,%1x,%1e",&x,&y,&z)；√
B.scanf(“%2d*%d%1f”,&x,&y,&z)；√
C.scanf(“%x%*d%o”,&x,&y)；√
D.scanf(“%x%o%6.2f",&x,&y,&z)；×
```

A:

```CPP
B：只能获取x的值，y和z都不能正常获取，但是不会报错。
C：*格式，表示跳过此数据不读入。放在%与格式d（或者s，c等）之间。 
D：scanf中的格式控制符不能指明浮点数的精度，D中%6.2f是错误的。 
```

Q:

```CPP

```

A:

```CPP

```

Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
#incude <stdio.h>
main()
｛
    FILE*fp； 
    int i,a[6]={1,2,3,4,5,6}；
    fp=fopen("d2.dat","w+")；
    for(i=0；i<6;i++)
        fprintf(fp,"%d\n",a [i])；
    rewind(fp）；
    for(i=0；i<6;i++)
        fscanf( fp,"%d",&a[5-i])；
    fclose(fp）；
    for(i=0；i<6；i++)
        printf("%d,",a [i])；//6,5,4,3,2,1,
}
```
A:
```CPP
[解]
fprintf是C/C++中的一个格式化写—库函数，位于头文件<stdio.h>中，其作用是格式化输出到一个流/文件中
for(i=0；i<6;i++)
    fprintf(fp,"%d\n",a [i])；  //这句是把a[i]依次写入文件流指针fp  内容为1 2 3 4 5 6

rewind(fp）//把文件流指针从新指向文件的开头

fscanf 其功能为根据数据格式(format)从输入流(stream)中写入数据(argument)
for(i=0；i<6;i++)
    fscanf( fp,"%d",&a[5-i])；//把文件流指针fp依次指向的内容写入到a[5-i],a[5]=1,a[4]=2,a[3]=3,a[2]=4,a[1]=5,a[0]=6;

```

Q:
```CPP
从控制台输入以","分隔的字符串，要求把字符串存入数组中。
```
A:
```CPP 
[解]
#include <iostream>
#include <sstream>
#include <vector>
#include <array>
 
int main()
{
	string str;
	getline(cin, str);
	istringstream input(str);
	vector<array<char, 100>> v;

	// 注意：下列循环在从  getline() 返回的流上的 
	// std::ios_base::operator bool() 返回 false 时终止
	for (array<char, 100>a; input.getline(&a[0], 100, '，'); ) {
		v.push_back(a);
	}

	for (auto& a : v) {
		std::cout << &a[0] << '\n';
	}

	return 0;
}

[扩展]
basic_istream& getline( char_type* s, std::streamsize count, char_type delim );//从流释出字符到数组，遇到行尾、指定的分隔符 delim、超过数组容量时停止。

int main()
{
	string str;
	getline(cin, str);
	std::istringstream input(str);
	std::vector<int> v;
	char a[100];
        while(input.getline(&a[0], 100, ','))
		v.push_back(atoi(a));

	for (auto a : v) {
		std::cout <<a << '\n';
	}

	return 0;
}
```


# 3. 类设计者的工具
### 类
#### union
Q:

```CPP

```

A:

```CPP

```

Q:

```CPP
若有以下说明和定义
union  dt
{
int  a;   char  b;  double  c;
}data;
以下叙述中错误的是 (    C  ) 。 
A.data的每个成员起始地址都相同。√
B.变量data所占内存字节数与成员c所占字节数相等。√
C.程序段：data.a=5;printf("%f\n",data.c);输出结果为5.000000。×
D.data可以作为函数的实参。√
```
A:
```CPP
printf函数只是将内存中的数据以不同的类型输出，而不能将内存中的整型数据自动转换为等值的浮点数，故C是错误的。在内存中，实数与整数的存放形式完全不一样，共用体的成员共用的是同一块内存，而不是同一个数值，因此选项C是错误的。

浮点数存储格式为IEEE754标准，以32位float(4字节)为例，float  i = 21.375；
i在计算机存储分为三个部分，
- 符号位S
- 阶码E,阶码E用偏移值表示
- 尾码M,尾码M用原码表示
```

![](https://uploadfiles.nowcoder.com/images/20180417/9486921_1523978479970_2ACE7E0DDC324F941D9408346EC00132)

```cpp
S为符号位，1代表负数，0代表整数，E代表偏移127的幂，M代表尾数。
由于i<0,所以S = 1,
对于E ,i 的二进制表示为10101.011，然后二进制右移4位，让最左边保留一位1，即10101.011 = 1.0101011*2^4,那么E = 127+4 = 131=10000011,
对于M，就是小数点后的的数0101011，由于要保证位23位，所以在末尾补0，即M=0101011000000000000000
所以i的存储为S+ E+M= 1-10000011-01010110000000000000000.
```

#### class

Q:

```CPP

```

A:

```CPP

```

Q:下面程序的输出是什么？

```CPP
#include <iostream>
using namespace std;
class parent
{
	int i;//未声明访问权限则默认private,它和结构体的唯一区别
protected:
	int x;//子类可以访问x,不能访问i
public:
	parent() { x = 0; i = 0; }
	void change() { x++; i++; }
	void display();
};
class son :public parent
{
public:
	void modify();
};
void parent::display() { cout << "x=" << x << endl; }
void son::modify() { x++; }
int main()
{
	son A; //A.x=0 ; A.i=0
	parent B;//B.x=0;B.i=0
	A.display();//x=0
	A.change();//执行parent::change().A.x=1; A.i=1
	A.modify();//执行son::modify().A.x=2; A.i=1
	A.display();//x=2
	B.change();//执行parent::change().B.x=1; B.i=1
	B.display();//x=1 
}
```

A:

```CPP
输出：x=0 x=2 x=1

- 子类会继承父类除构造函数和析构函数之外[所有的数据成员和成员函数]。
虽然son不能继承parent的构造函数,但初始化A时强制调用了parent的构造函数。
- 实例化后，[子类和父类的对象互不影响]。
- 类的成员变量[未声明访问权限则默认private]，这是它和结构体的唯一区别。
- 基类的private成员不能被子类访问，但其protected成员可以被子类访问，两者均不可在类外访问，故文中子类可以访问x,不能访问i
```



#####  static members

Q:

```CPP
关于类的静态成员的描述：
A. 静态成员不属于对象，是类的共享成员。√
B. non-const static数据成员要在类外定义和初始化,const static的数据成员必须在类里初始化。√
C. 静态成员函数不拥有this指针，需要通过类参数访问对象成员。√
D. 只有静态成员函数可以操作静态数据成员。×
```

A:

```CPP

```

Q:

```CPP

```

A:

```CPP

```





### 重载 

#### 普通函数的重载
Q1:
```cpp
下列对重载函数的描述中，_____A_______是错误的。
A.重载函数中不允许使用默认参数.
B.重载函数中编译时根据参数表进行选择
C.构造函数重载将会给初始化带来多种方式
D.不要使用重载函数来描述毫无相干的函数
```
A1:
```CPP
一般情况下，在函数调用时形参从实参那里取得值，因此实参的个数应于形参相同。
有时多次调用同样的实参，c++提供简单的处理办法，给实参一个默认值，这样形参就不必从实参取值了。
如有一函数申明float area (float r=6.5)；指定r的默认值为6.5。
如果在调用此函数时，确认r的值为6.5，则可以不必给出实参的值，如： area( )；  //相当于area（6.5）；
如果不想使形参取默认值，则通过实参另行给出。如：area(7.5)。
```
Q2:
```CPP
以下不是double compare(int,int)的重载函数的是()
正确答案: D   你的答案: D (正确)
int compare(double,double)
double compare(double,double)
double compare(double,int)
int compare(int,int)
```
A2:
```CPP
重载函数与类型无关，所以不看类型。
- 成员函数被重载的特征：
    1. 相同的范围（在同一个类中）；
    2. 函数名字相同；
    3. 参数不同；
    4. virtual 关键字可有可无。
- 覆盖是指派生类函数覆盖基类函数，特征是：
    1. 不同的范围（分别位于派生类与基类）；
    2. 函数名字相同；
    3. 参数相同；
    4. 基类函数必须有 virtual 关键字。
- 隐藏是指派生类的函数屏蔽了与其同名的基类函数，规则如下：
    1. 如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无 virtual 关键字，基类的函数将被隐藏（注意别与重载混淆）。
    2. 如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有 virtual 关键字。此时，基类的函数被隐藏（注意别与覆盖混淆）。
```

#### 运算符重载
Q:

```CPP
有关运算符重载正确的描述是（D）
正确答案: D   你的答案: A (错误)
c++语言允许在重载运算符时改变运算符的操作个数
c++语言允许在重载运算符时改变运算符的优先级
c++语言允许在重载运算符时改变运算符的结合性
c++语言允许在重载运算符时改变运算符原来的功能
```

A:

```CPP
c ++语言允许在重载运算符时改变运算符原来的功能。例如将“ + + "符号重载时，可以定义为“--"的功能。但是，不提倡这样做，重载运算符最好仍保持原有的功能。
与普通函数的重载规则不同，这里的对象是运算符（运算符重载是特殊的函数重载），运算符重载有自己的规则，不要混为一谈。
```

Q:

```CPP

```

A:

```CPP

```

Q:

```CPP

```

A:

```CPP

```

Q:

```CPP

```
A:
```CPP

```

### 动态绑定
#### 纯虚函数
Q1:
```CPP
关于纯虚函数，下列表述正确的是？（ B  ）
正确答案: B   你的答案: C (错误)
A.派生类必须实现基类的纯虚函数
B.纯虚函数的声明总是以“=0”结束
C.纯虚函数是给出实现版本（即无函数体定义）的虚函数
D.含有纯虚函数的类一定是派生类
```
A1:
```cpp
A.含有纯虚函数的类是抽象类，对于继承抽象类的派生类来说，如果[派生类实现了基类的纯虚函数，则派生类可以实例化]。若派生类没有实现该纯虚函数，则该[派生类也是抽象类]，即不能实例化。
C.纯虚函数是需要在该类下不实现而[在派生类中实现]的函数。
D.含有纯虚函数的类一定是[基类]，需要[派生类]来[实现其虚函数的功能]。
```
Q2:
```CPP
如果不使用多态机制，那么通过基类的指针虽然可以指向派生类对象，但是只能访问从基类继承的成员。
正确答案: A   你的答案: A (正确)
A.是
B.否
```
A2:
```CPP
若不使用多态机制，则动态绑定不存在。即基类与派生类虚函数无法根据指针指向的对象动态选择。 虚函数就无法使用。
```





### 面向对象
Q:
```CPP
在C++面向对象编程语言中，以下阐述 
A.接口中可以用虚方法。×
B.一个类可以实现多个接口。√
C.接口不能被实例化。√
D.接口中可以包含已经实现的方法。×
```
A:
```CPP
接口是指[只包含纯虚函数]的[抽象类]。
A:接口和普通的抽象类不一样。所以A不对，必须是纯虚函数。
D:接口即只包含纯虚函数的抽象类，所以D是不对的。
```
Q:

```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

​```Q:

​```CPP

```
A:
```CPP

```
Q:
```CPP

```
A:
```CPP

```



