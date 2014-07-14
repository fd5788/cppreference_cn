#排序（std::sort）

定义于头文件`<algorithm>`中：

```C++
template< typename RandomIt >
void sort( RandomIt first, RandomIt last );    (1)
```
```C++
template< typename RandomIt, typename Compare >
void sort( RandomIt first, RandomIt last, Compare comp );       (2)
```

升序排序区间`[first, last)`里的元素，注意不包括`last`所指向的元素。不保证相等元素的先后顺序，即属不稳定排序。版本(1)用`<`排序，即默认为升序，版本(2)用自定义的函数`cmp`。

##参数

`first, last` —— 待排序的元素的区间

`comp` —— 比较函数，若第一个参数小于第二个则返回`true`。
       比较函数必须使用下面的等效声明：

`bool cmp(const Type1 &a, const Type2 &b);`

比较函数不一定非得声明为`const &`，但是这个函数对象应该不能修改传递过来的参数。

类型`Type1`和`Type2`必须是能够解除引用操作和隐式互转的`RandomIt`类型。

**类型约束**
- `RandomIt`必须满足[值可交换](../concept/ValueSwappable.md)和[随机访问迭代器](http://en.cppreference.com/w/cpp/concept/RandomAccessIterator)的要求。
- 解除引用的`RandomIt`类型必须满足[可移动](../concept/MoveAssignable.md)和[可移动构造](../concept/MoveConstructible.md)。
- 比较函数必须满足[Compare](http://en.cppreference.com/w/cpp/concept/Compare)的要求。

##返回值
无返回值。

##复杂度

O(N*log(N)), 其中`N = std::distance(first, last)`为平均比较次数。( - C++11)

O(N*log(N)), 其中`N = std::distance(first, last)`为比较次数。(C++11 - )

##例子

```C++
#include <algorithm>
#include <functional>
#include <array>
#include <iostream>

int main()
{
    std::array<int, 10> s = {5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

    // sort using the default operator<
    std::sort(s.begin(), s.end());
    for (int a : s) {
        std::cout << a << " ";
    }
    std::cout << '\n';

    // sort using a standard library compare function object
    std::sort(s.begin(), s.end(), std::greater<int>());
    for (int a : s) {
        std::cout << a << " ";
    }
    std::cout << '\n';

    // sort using a custom function object
    struct {
        bool operator()(int a, int b)
        {
            return a < b;
        }
    } customLess;
    std::sort(s.begin(), s.end(), customLess);
    for (int a : s) {
        std::cout << a << " ";
    }
    std::cout << '\n';

    // sort using a lambda expression
    std::sort(s.begin(), s.end(), [](int a, int b) {
        return b < a;
    });
    for (int a : s) {
        std::cout << a << " ";
    }
    std::cout << '\n';
}
```

输出为：

```C++
0 1 2 3 4 5 6 7 8 9
9 8 7 6 5 4 3 2 1 0
0 1 2 3 4 5 6 7 8 9
9 8 7 6 5 4 3 2 1 0
```

##另见

- [partial_sort](partial_sort.md)   按一定顺序排出区间里的前N个元素（函数模板）
- [stable_sort](stable_sort.md)     稳定排序，即排序时保持相等元素的先后次序（函数模板）
