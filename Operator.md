#表4.4：运算符优先级
| 结合律 | 运算符        | 功能               | 用法                    |
| ------ | ------------- | ------------------ | ----------------------- |
| 1级    | 作用域        |                    |                         |
| 左     | ::            | 全局作用域         | ::name                  |
| 左     | ::            | 类作用域           | class::name             |
| 左     | ::            | 命名空间作用域     | namespace::name         |
| 2级    |               |                    |                         |
| 左     | .             | 成员选择           | object.member           |
| 左     | ->            | 成员选择           | pointer->member         |
| 左     | []            | 下标               | expr[expr]              |
| 左     | ()            | 函数调用           | name(expr_list)         |
| 左     | ()            | 类型构造           | type(expr_list)         |
| 3级    |单目运算符               |                    |                         |
| 右     | ++            | 后置递增运算       | lvalue++                |
| 右     | --            | 后置递减运算       | lvalue--                |
| 右     | typeid        | 类型ID             | typeid(type)            |
| 右     | typeid        | 运行时类型ID       | typeid(expr)            |
| 右     | explicit cast | 类型转换           | *cast_name*<type>(expr) |
| 4级    |               |                    |                         |
| 右     | ++            | 前置递增运算       | ++lvalue                |
| 右     | --            | 前置递减运算       | --lvalue                |
| 右     | ~             | 位求反             | ~expr                   |
| 右     | !             | 逻辑非             | !expr                   |
| 右     | -             | 一元负号           | -expr                   |
| 右     | +             | 一元正号           | +expr                   |
| 右     | *             | 解引用             | *expr                   |
| 右     | &             | 取地址             | &lvalue                 |
| 右     | ()            | 类型转换           | (type) expr             |
| 右     | sizeof        | 对象的大小         | sizeof expr             |
| 右     | sizeof        | 类型的大小         | sizeof( type )          |
| 右     | Sizeof…       | 参数包的大小       | sizeof...( name )       |
| 右     | new           | 创建对象           | new type                |
| 右     | new[]         | 创建数组           | new type[size]          |
| 右     | delete        | 释放对象           | delete expr             |
| 右     | delete[]      | 释放数组           | delete[] expr           |
| 右     | noexcept      | 能否抛出异常       | noexcept ( expr )       |
| 5级    |               |                    |                         |
| 左     | ->*           | 指向成员选择的指针 | ptr->*ptr_to_member     |
| 左     | .*            | 指向成员选择的指针 | obj.*ptr_to_member      |
| 6级    | 算术运算符    |                    |                         |
| 左     | *             | 乘法               | expr * expr             |
| 左     | /             | 除法               | expr / expr             |
| 左     | %             | 取模（取余）       | expr % expr             |
| 7级    |               |                    |                         |
| 左     | +             | 加法               | expr + expr             |
| 左     | -             | 减法               | expr - expr             |
| 8级    | 移位          |                    |                         |
| 左     | <<            | 向左移位           | expr << expr            |
| 左     | >>            | 向右移位           | expr >> expr            |
| 9级    | 关系运算符    |                    |                         |
| 左     | <             | 小于               | expr < expr             |
| 左     | <=            | 小于等于           | expr <= expr            |
| 左     | >             | 大于               | expr > expr             |
| 左     | >=            | 大于等于           | expr >= expr            |
| 10级   |               |                    |                         |
| 左     | ==            | 相等               | expr == expr            |
| 左     | !=            | 不相等             | expr != expr            |
| 11级   | 位运算        |                    |                         |
| 左     | &             | 位与               | expr & expr             |
| 12级   |               |                    |                         |
| 左     | ^             | 位异或             | expr ^ expr             |
| 13级   |               |                    |                         |
| 左     | \|            | 位或               | expr \| expr            |
| 14级   | 逻辑运算符    |                    |                         |
| 左     | &&            | 逻辑与             | expr && expr            |
| 15级   |               |                    |                         |
| 左     | \|\|          | 逻辑或             | expr \|\| expr          |
| 16级   |               |                    |                         |
| 右     | ? :           | 条件               | expr ? expr : expr      |
| 17级   | 赋值运算符    |                    |                         |
| 右     | =             | 赋值               | lvalue = expr           |
| 18级   |               |                    |                         |
| 右     | *=, /=, %=    | 复合赋值           | lvalue += expr等        |
| 右     | +=, -=        |                    |                         |
| 右     | <<=, >>=      |                    |                         |
| 右     | &=, \|=, ^=   |                    |                         |
| 19级   |               |                    |                         |
| 右     | throw         | 抛出异常           | throw expr              |
| 20级   |               |                    |                         |
| 左     | ,             | 逗号               | expr, expr              |