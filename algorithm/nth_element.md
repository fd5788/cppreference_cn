#部分排序（std::nth_element）

定义于头文件`<algorithm>`中：

```C++
template< typename RandomIt >
void nth_element( RandomIt first, RandomIt nth, RandomIt last );    (1)
```
```C++
template< typename randomIt, typename Compare >
void nth_element( RandomIt first, RandomIt nth, RandomIt last, Compare comp );       (2)
```

`nth_element`是一个部分排序算法，它像这样重新排列区间`[first, last)`里的元素：

- `nth`所指向的元素变成`[first, last)`排好序后该位置的元素。
- 新的`nth`之前的所有元素小于或等于`nth`之后的元素。

更一般地，`nth_element`部分地对区间`[first, last)`里的元素按升序排序，直至对于区间`[first, nth)`里的任何元素`i`和区间`[nth, last)`里的任何元素`j`，条件`!(*j < *i)`（版本2则为`comp(*j, *i) == false`）满足。`nth`位置的元素恰为若该区间全排序时第`n`小（或大）的元素。

`nth`可能为`end`迭代器，这种情况对函数无影响。

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

平均时间复杂度为线性于`std::distance(first, last)`。

##注意

该算法通常是一个[introselect](http://en.wikipedia.org/wiki/Introselect)算法，尽管也允许使用其他平均性能也不错的[selection algorithms](http://en.wikipedia.org/wiki/Selection_algorithm)算法。

##例子

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
 
int main()
{
    std::vector<int> v{5, 6, 4, 3, 2, 6, 7, 9, 3};
 
    std::nth_element(v.begin(), v.begin() + v.size()/2, v.end());
    std::cout << "The median is " << v[v.size()/2] << '\n';
 
    std::nth_element(v.begin(), v.begin()+1, v.end(), std::greater<int>());
    std::cout << "The second largest element is " << v[1] << '\n';
}
```

输出为：

```C++
The median is 5
The second largest element is 7
```

##另见

- [partial_sort_copy](partial_sort_copy.md)   按一定顺序排出区间里的前N个元素
- [stable_sort](stable_sort.md)    对区间的元素进行排序，同时保持相等元素的先后顺序
- [sort](stable_sort.md)     递增排序，且不一定会保持相等元素的先后次序
