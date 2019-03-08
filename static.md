# 类定义中的static
1.在类定义中，static声明不绑定到对象的实例。在类外定义中，static表示的是存储器，见stack和heap内存管理。

2.只能在声明中使用，不能在定义中使用。
```cpp
class X{static int n;};//声明中用static
int X::n=1;//定义中不用static
```

3.两种声明语法：
- 声明static data members：脱离了对象这个概念，比如利率。
- 声明static member functions：没有this指针，只能处理static 数据。


4.non-static member functions：有一个不可以写出来的隐藏参数指针this，代表被调用对象的地址。
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

5.调用静态成员函数的方式
- 通过类名调用。
- 通过静态对象调用。

```cpp
class Account
{
    public:
    static double m_rate;//静态数据变量
    static void set_rate(const double& x)//静态成员函数
    {
        m_rate = x;
    }
};
double Account::m_rate = 8.0;
int main()
{
    Account::set_rate(5.0);//通过类名调用
    Account a;
    a.set_rate(7.0);//通过对象调用
}
```