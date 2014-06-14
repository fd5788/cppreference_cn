##强制转化为右值（std::move)（[en](http://en.cppreference.com/w/cpp/utility/move)）

定义于头文件`<utility>`（[en](http://en.cppreference.com/w/cpp/header/utility)）中：

```C++
template< class T >
typename std::remove_reference<T>::type&& move( T&& t ) noexcept ;              (C++11 - C++14)
```
```C++
template< class T >
constexpr typename std::remove_reference<T>::type&& move( T&& t ) noexcept ;    (C++14 - )
```

`std::move`返回指向实参的右值引用，同时转化实参为将亡值（`xvalue`，`eXpiring Value`[en](http://en.cppreference.com/w/cpp/language/value_category)）。

接受`xvalue`类型的代码有机会优化不必要的额外开销，通过窃取实参的数据（并未移动数据，只是攻城掠地），而从导致其处于有效（生命期尚未结束）但未指定的状态。

##参数

`t` - 待移动的对象

##返回值

`static_cast<typename std::remove_reference<T>::type&&>(t)`

##异常

`noexcept`指定： `noexcept`

即不允许抛出异常。

##注意

在重载选择时，若实参为右值引用类型（无论是形如临时对象这样的纯右值（`prvalues`）还是如`std::move`转化而来的将亡值（`xvalues`）），则形参为右值引用类型的函数（包括移动构造函数（[move constructors](../language/move_constructor.md)）、移动赋值运算符（[move assignment operators](../language/move_operator.md)）和形如`std::vector::push_back`这样的普通的成员函数）会获得匹配。如果实参为资源独占型对象，重载有选择窃取（`move`）实参所持有的资源的权利，但这不是必须的，即不一定会这样做。例如，链表类型的移动构造函数可能会复制指针并赋给链表头结点，然后把实参结点置为`nullptr`，而不是分配空间并逐个复制结点。

右值引用同时又是左值（`lvalues`），而且必须先转化为`xvalues`才能传递给接受右值引用参数的函数以便正确重载。这就是为什么移动构造函数和移动赋值操作符常常需要使用`std::move`的原因：

```C++
// Simple move constructor
Foo(A&& arg) : member(std::move(arg)) // the expression "arg" is lvalue
{}
// Simple move assignment operator
A& operator=(A&& other) {
     member = std::move(other.member);
     return *this;
}
```

有个例外，那就是当函数的参数类型是指向类型模板参数（全局引用）的右值引用时，需要用[std:forward](forward.md)替代之。

除非有另外说明，否则所有接受右值引用形参的标准库函数（如`std::vector::push_back`）都将保证所移动的实参处于有效但未指定的状态（也就是说，当一个对象被移动到标准库容器后，只有没有前提条件的运算符（如赋值运算符）才可以作用于该对象）：

```C++
td::vector<std::string> v;
std::string str = "example";
v.push_back(std::move(str)); // str is now valid but unspecified
str[0]; // undefined behavior: operator[](size_t n) has a precondition size() > n
str.clear(); // OK, clear() has no preconditions
```

此外，通过`xvalue`型实参调用的标准库函数可能会假设该实参是该对象的唯一别名。如果它是左值引用通过`std::move`转换而来，也不会有别名检查。特别地，这意味着标准库的移动赋值运算符不必非得进行自我赋值的检查。例如，下面的代码会导致为定义行为：

```C++
std::string s = "example";
s = std::move(s); // undefined behavior
```

##例子

```C++
#include <iostream>
#include <utility>
#include <vector>
#include <string>

int main()
{
    std::string str = "Hello";
    std::vector<std::string> v;

    // uses the push_back(const T&) overload, which means
    // we'll incur the cost of copying str
    v.push_back(str);
    std::cout << "After copy, str is \"" << str << "\"\n";

    // uses the rvalue reference push_back(T&&) overload,
    // which means no strings will be copied; instead, the contents
    // of str will be moved into the vector.  This is less
    // expensive, but also means str might now be empty.
    v.push_back(std::move(str));
    std::cout << "After move, str is \"" << str << "\"\n";

    std::cout << "The contents of the vector are \"" << v[0]
                                         << "\", \"" << v[1] << "\"\n";

    // string move assignment operator is often implemented as swap,
    // in this case, the moved-from object is NOT empty
    std::string str2 = "Good-bye";
    std::cout << "Before move from str2, str2 = '" << str2 << "'\n";
    v[0] = std::move(str2);
    std::cout << "After move from str2, str2 = '" << str2 << "'\n";
}
```

一种可能的输出为：

```C++
After copy, str is "Hello"
After move, str is ""
The contents of the vector are "Hello", "Hello"
Before move from str2, str2 = \'Good-bye\'
After move from str2, str2 = \'Hello\'
```

##复杂度

常量级

##另见

- [forward](forward.md)(C++11)                      转发函数实参（函数模板）
- [move_if_noexcept](move_if_noexcept.md)(C++11)    如果移动构造函数不跑出异常就强制转化为右值引用（函数模板）
- [move](../algorithm/move.md)(C++11)               移动区间里的元素到另一个区间（函数模板）
