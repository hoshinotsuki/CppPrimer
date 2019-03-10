# 1. C++基础
## 表达式
### sizeof
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

4.ex4_28:输出每种空间内置类型大小
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

```


## 字符串、向量、数组
### 字符串

### 数组
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
## 声明
### const
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
# 2. C++标准库
### IO库
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
### 重载 
Q:
```cpp
下列对重载函数的描述中，_____A_______是错误的。
A.重载函数中不允许使用默认参数.
B.重载函数中编译时根据参数表进行选择
C.构造函数重载将会给初始化带来多种方式
D.不要使用重载函数来描述毫无相干的函数
```
A:
```CPP
一般情况下，在函数调用时形参从实参那里取得值，因此实参的个数应于形参相同。
有时多次调用同样的实参，c++提供简单的处理办法，给实参一个默认值，这样形参就不必从实参取值了。
如有一函数申明float area (float r=6.5)；指定r的默认值为6.5。
如果在调用此函数时，确认r的值为6.5，则可以不必给出实参的值，如： area( )；  //相当于area（6.5）；
如果不想使形参取默认值，则通过实参另行给出。如：area(7.5)。
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



