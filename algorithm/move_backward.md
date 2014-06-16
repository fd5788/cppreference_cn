#逆向移动（std::move_backward）（[en](http://en.cppreference.com/w/cpp/header/algorithm/move_backward)）

定义于头文件`<algorithm>`（[en](http://en.cppreference.com/w/cpp/header/algorithm)）中：

```C++
template< typename BidirIt1, typename BidirIt2 >
BidirIt2 move( BidirIt1 first, BidirIt1 last, BidirIt2 d_last );    (C++11 - )
```

移动区间 [first, last) 之间的元素到另一个以 d_last 结尾的区间。虽然元素是逆向移动的（最后一个元素先移动），但它们的相对顺序保持不变。

如果 d_last 在区间 [first, last)之间，将导致未定义行为，如此必须用[std::move](move.md)替代之。

##参数

first, last - 待移动元素区间
    d_first - 目标区间的终止位置。

###类型要求

- `BidirIt1, BidirIt2`必须满足`BidirectionalIterator`（[en](http://en.cppreference.com/w/cpp/concept/BidirectionalIterator)）的要求。

##返回值
目标区间的迭代器，指向所移动的最后一个元素。

##复杂度

last-first次移动赋值的代价。

##可能的实现

```C++
template< typename BidirIt1, typename BidirIt2 >
BidirIt2 move_backward(BidirIt1 first,
                                     BidirIt1 last,
                                     BidirIt2 d_last)
{
    while (first != last) {
        *(--d_last) = std::move(*(--last));
    }
    return d_last;
}
```

##注意

移动重叠区间时，如果目标区间的起始位置不在源区间内，那么用 std::move 移动到目标区间左侧是合适的；如果目标区间的终止位置不在源区间内，那么用 std::move_backward 移动到目标区间右侧亦是合适的。

##例子

下面的例子是把本身不可复制的线程对象从一个容器移动到另一个容器。

```C++
#include <algorithm>
#include <vector>
#include <string>
#include <iostream>

int main()
{
    std::vector<std::string> src{"foo", "bar", "baz"};
    std::vector<std::string> dest(src.size());

    std::cout << "src: ";
    for (const auto &s : src)
    {
        std::cout << s << ' ';
    }
    std::cout << "\ndest: ";
    for (const auto &s : dest)
    {
        std::cout << s << ' ';
    }
    std::cout << '\n';

    std::move_backward(src.begin(), src.end(), dest.end());

    std::cout << "src: ";
    for (const auto &s : src)
    {
        std::cout << s << ' ';
    }
    std::cout << "\ndest: ";
    for (const auto &s : dest)
    {
        std::cout << s << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

输出为：

```C++
src: foo bar baz
dest:
src:
dest: foo bar baz
```

##另见

- [move](move.md)（C++11）           移动区间里的元素到另一个新的位置（函数模板）
