##未抛出常时强制转换为右值（std::move_if_noexcept)

定义于头文件`<utility>`（[en](http://en.cppreference.com/w/cpp/header/utility)）中：

```C++
template<typename T >
typenamestd::conditional<
!std::is_nothrow_move_constructible<T>::value && std::is_copy_constructible<T>::value,
const T&,
T&&
>::typemove_if_noexcept(T& x) noexcept ;                                                                (C++11 - C++14)
```

```C++
template<typename  T >
constexprtypename std::conditional<
!std::is_nothrow_move_constructible<T>::value && std::is_copy_constructible<T>::value,
const T&,
T&&
>::typemove_if_noexcept(T& x) noexcept ;                                                                (C++14 - )
```

如果移动构造函数不抛出异常，`move_if_noexcept`返回指向实参的右值引用，否则返回指向实参的左值引用。这是移动语义和强异常保证联合使用的典范。

##参数

`x`- 待移动或复制的对象

##返回值

`std::move(x)`或`x`，依赖于异常保证。

##异常

`noexcept`指定：`noexcept`

即不允许抛出异常。

##注意

`move_if_noexcept`用于那些可能不得不分配新的空间然后从原有的空间移动或者复制到新分配的空间的情形，如调用`std::vecotr::resize`。如果进行该操作时抛出异常，`std::vector::resize`将会撤销它此前做的所有事情，而这只有在用`std::move_if_noexcept`来决定是使用移动构造还是复制构造函数时才可能做得到。（除非没有定义复制构造函数，这样的话将使用移动构造，并且强异常得不到保证。）

##例子

```C++
#include<iostream>
#include<utility>

structBad
{
    Bad() {}
    Bad(Bad&&)  // may throw
    {
        std::cout << "Throwing move constructor called\n";
    }
    Bad(const Bad&) // may throw as well
    {
        std::cout << "Throwing copy constructor called\n";
    }
};

structGood
{
    Good() {}
    Good(Good&&) noexcept // will NOT throw
    {
        std::cout << "Non-throwing move constructor called\n";
    }
    Good(const Good&) noexcept // will NOT throw
    {
        std::cout << "Non-throwing copy constructor called\n";
    }
};

int main()
{
    Good g;
    Bad b;
    Good g2 = std::move_if_noexcept(g);
    Bad b2 = std::move_if_noexcept(b);

    return 0;
}
```

输出为：

```C++
Non-throwingmove constructor called
Throwingcopy constructor called
```

##复杂度

常量级

##另见

- [forward](forward.md)(C++11)                      转发函数实参（函数模板）
- [move](move.md)(C++11)                            强制转换为右值引用（函数模板）
