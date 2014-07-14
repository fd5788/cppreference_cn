#部分排序（std::partial_sort）

定义于头文件`<algorithm>`中：

```C++
template< typename InputIt, typename RandomIt >
RandomIt partial_sort_copy( InputIt first, InputIt last, RandomIt d_first, RandomIt d_last );    (1)
```
```C++
template< typename InputIt, typename RandomIt, Compare comp>
RandomIt partial_sort_copy(InputIt first, InputIt last, RandomIt d_first, RandomIt d_last, Compare comp );       (2)
```

升序排列区间`[first, last)`里的元素，结果保存于区间`[d_first, d_last)`。

最多`d_last - d_first`个元素移至区间`[d_first, d_first + n)`，然后开始排序，其中`n`为带排序元素个数，`n = min(last - first, d_last - d_first))`。不保证是否保持相等元素的先后顺序。区间`[middle, last)`的元素的顺序未知。版本(1)用`<`排序，即默认为升序，版本(2)用自定义的函数`cmp`。

##参数

`first, last` —— 待排序的元素的区间

`comp` —— 比较函数，若第一个参数小于第二个则返回`true`。
       比较函数必须使用下面的等效声明：

`bool cmp(const Type1 &a, const Type2 &b);`

比较函数不一定非得声明为`const &`，但是这个函数对象应该不能修改传递过来的参数。

类型`Type1`和`Type2`必须是能够解除引用操作和隐式互转的`RandomIt`类型。

**类型约束**
- `InputIt` 必须满足输入迭代器（InputIterator）。
- `RandomIt`必须满足[值可交换](../concept/ValueSwappable.md)和[随机访问迭代器](http://en.cppreference.com/w/cpp/concept/RandomAccessIterator)的要求。
- 解除引用的`RandomIt`类型必须满足[可移动](../concept/MoveAssignable.md)和[可移动构造](../concept/MoveConstructible.md)。

##返回值
指向已排序元素区间的上界的迭代器。

##复杂度

O(N*log(min(D,N)),其中`N = std::distance(first, last), D = std::distance(d_first, d_last)`次`cmp`函数调用。

##例子

下面是对整型`vector`排序，然后把它们复制到一个比它小和比它大的`vector`中的例子。

```C++
#include <algorithm>
#include <vector>
#include <functional>
#include <iostream>
 
int main()
{
    std::vector<int> v0{4, 2, 5, 1, 3};
    std::vector<int> v1{10, 11, 12};
    std::vector<int> v2{10, 11, 12, 13, 14, 15, 16};
    std::vector<int>::iterator it;
 
    it = std::partial_sort_copy(v0.begin(), v0.end(), v1.begin(), v1.end());
 
    std::cout << "Writing to the smaller vector in ascending order gives: ";
    for (int a : v1) {
        std::cout << a << " ";
    }
    std::cout << '\n';
    if(it == v1.end())
        std::cout << "The return value is the end iterator\n";
 
    it = std::partial_sort_copy(v0.begin(), v0.end(), v2.begin(), v2.end(), 
                                std::greater<int>());
 
    std::cout << "Writing to the larger vector in descending order gives: ";
    for (int a : v2) {
        std::cout << a << " ";
    }
    std::cout << '\n' << "The return value is the iterator to " << *it << '\n';
}
```

输出为：

```C++
Writing to the smaller vector in ascending order gives: 1 2 3
The return value is the end iterator
Writing to the larger vector in descending order gives: 5 4 3 2 1 15 16
The return value is the iterator to 15
```

##另见

- [partial_sort_copy](partial_sort_copy.md)   按一定顺序排出区间里的前N个元素
- [sort](stable_sort.md)     递增排序，且不一定会保持相等元素的先后次序
- [stable_sort](stable_sort.md)    对区间的元素进行排序，同时保持相等元素的先后顺序