#部分排序（std::partial_sort）

定义于头文件`<algorithm>`中：

```C++
template< typename RandomIt >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last );    (1)
```
```C++
template< typename RandomIt, typename Compare >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last, Compare comp );       (2)
```

重新排列区间`[first, last)`里的元素，区间`[first, middle)指向排序后的`middle-first`个元素。

不保证是否保持相等元素的先后顺序。区间`[middle, last)`的元素的顺序未知。版本(1)用`<`排序，即默认为升序，版本(2)用自定义的函数`cmp`。

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

##返回值
无返回值。

##复杂度

约为`(last-first)log(middle-first)`次`cmp`函数调用。

##例子

```C++
#include <algorithm>
#include <functional>
#include <array>
#include <iostream>
 
int main()
{
    std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
 
    std::partial_sort(s.begin(), s.begin() + 3, s.end());
    for (int a : s) {
        std::cout << a << " ";
    } 
}
```

输出为：

```C++
0 1 2 7 8 6 5 9 4 3
```

##另见

- [nth_element](nth_element.md)    在确保区间被给定元素分开时对其部分排序（函数模板）
- [partial_sort_copy](partial_sort_copy.md)   按一定顺序排出区间里的前N个元素（函数模板）
- [stable_sort](stable_sort.md)    对区间的元素进行排序，同时保持相等元素的先后顺序（函数模板）
- [sort](stable_sort.md)     递增排序，且不一定会保持相等元素的先后次序（函数模板）
