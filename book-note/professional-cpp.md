# C++20高级编程

本书分为6个part。

## Part1, Introduction

1. 入门c++
2. c++字符串
3. 代码风格

### Chapter1

```c++
#include <iostream>
#include <format>

int main()
{
    std::cout << std::format("hello world!\n");
    return 0;
}
```
#### Namespace

命名空间解决的问题主要是不同代码片段（如你和他写的代码）带来的命名冲突。命名空间可以嵌套。

```c++
namespace Mylib {
    namespace Networking {
        namespace FTP {
            // ....
        }
    }
}

// namespace alias
namespace MyFTP = Mylib::Networking::FTP;
```
#### Numerical Limits

标准库提供了关于每种数值类型的极限信息，（最大，最小）

```c++
\\ 宏
INT_MAX
INT_MIN

\\ 标准库
#inlcude <limits>

numeric_limits<int>::max();
numeric_limits<int>::min();

numeric_limits<double>::max();
numeric_limits<double>::min(); // 正数
numeric_limits<double>::lowest(); // 负数

numeric_limits<dobule>::infinity();
```

#### Enumerated Type

旧风格的枚举类型并不是强类型，没有类型安全。因此， 使用下面新式（`enum class`）的枚举
```c++
enum class PieceType : unsigned long {
    King,
    Queen,
    Rook,
    Pawn
};
```

#### Structs

结构体类型通常作为封装一种或多种类型为一种新的类型。经典的用例就是数据库记录。

使用`Modules`作为接口。`*.cppm`作为模块的接口文件扩展。
```c++
// employee.cppm file
// 使用export关键字导出接口
export module employee;

 export struct Employee {
    char first_initial;
    char last_initial;
    int employee_number;
    int salary;
 };
```

```c++
// 使用module
import <iostream>;
import <format>;
import employee;

int main()
{
    Employee a;
    a.first_initial = 'J';
    a.last_initial = 'D';
    a.employee_number = 42;
    a.salary = 30000;

    return 0;
}
```

#### Three-way operation(`<=>` )

三路比较运算符用作决定两个值是什么顺序关系（全序，偏序）。对于一个表达式而言，它返回一个值是否等于，小于或者大于另一个值。 返回类枚举类型，`<compare>`。

对于基本（primitive）类型来说，三路操作符作用不大。然而，对于在比较一些对象的时候，会变得很有用（代码量减少）。

```c++
// 强序关系
// 操作数是整型
strong_ordering::less
strong_ordering::grater
strong_ordering::equal

// 偏序关系
// 操作数是浮点类型
partial_ordering::less
partial_ordering::greater
partial_ordering::equivalent
partial_ordering::unorderd  // 操作数不是一个数字

// 弱序关系
// 操作数是自定义类型
weak_ordering::less
weak_ordering::greater
weak_ordering::equavalent
```

#### 属性（Attributes)

属性作为一种将可选或第三方特定信息加入源码中的机制。

1. `[[nodiscard]]`

    编译器发出警告，如果函数返回值没有被使用
    ```c++
    [[nodiscard]]
    int func()
    {
        return 43;
    }

    int main()
    {
        func();
    }
    ```
2. `[[maybe_unused]]`
   
   当一些东西未使用，用来阻止编译器发出警告。使用在类和结构体，非静态数据成员，变量，函数，类型别名，联合体，枚举类型和值
    ```c++
    int func([[maybe_unused]] int param1) {
        return 42;
    }
    ```
3. `[[noreturn]]`
   
   作用在函数时，意味着它永远不会将控制权返还给调用点。
   ```c++
   [[noreturn]] 
   void forceProgramTerminate() {
       std::exit(1);
   }
   ```
4. `[[deprecated]]`

    标记某些东西已经被废弃，可以使用可选参数进行解释。
    ```c++
    [[disprecated("unsafe method, please use xyz")]] void func();
    ```

5. `[[likey]]`与`[[unlikely]]`

    帮助编译器优化代码。如根据它们的相似性标记`if`和`switch`的分支。（非常少用到，因为编译器很强大）


#### 标准库

##### `std::optional`

表示是否拥有某个指定值。**不能用于引用，但可以用于指针**

+ `nullopt`
+ `has_value()`
+ `value()`
+ `value_or()`

#### 初始化列表

使函数接收可变数量的参数更加容易

```c++
import <initializer_list>

int makeSum(std::initializer_list<int> values)
{
    int total {0};
    for (int value : values) {
        total += value;
    }
    return total;
}
```

#### 字符串

1. c-style: 字符数组
2. c++ sttle：字符串类

#### `Const`的使用

1. const as a Qualifier for a Type
    + const作用于指针(顶层和底层)
    + const作用于形参变量，使参数不能被修改（传参时，non-const可以转成const）
  
2. const Methods，阻止修改成员变量