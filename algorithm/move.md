#移动（std::move）

定义于头文件`<algorithm>`（[en](http://en.cppreference.com/w/cpp/header/algorithm)）中：

```C++
template< class InputIt, class OutputIt >
OutputIt move( InputIt first, InputIt last, OutputIt d_first );    (C++11 - )
```

移动区间 [first, last) 之间的元素到另一个以 d_first 打头的区间。移动后，[firest, last) 区间依然包含合适类型的有效值，但其值并不一定和移动前时一样。

##参数

first, last - 待移动元素区间
    d_first - 目标区间的起始位置。如果 d_first 在区间 [first, last) 之中, 则不能用[std::move](move.md)，而要用[std::move_backward](move_backward.md)替代之。

###类型要求

- `InputIt`必须满足`InputIterator`的要求。
- `OutputIt`必须满足`OutputIterator`的要求。

##返回值
输出迭代器，指向所移动的最后一个元素的下一个位置（d_first + (last - first)）。

##复杂度

last-first次移动赋值的代价。

##可能的实现

```C++
template<class InputIt, class OutputIt>
OutputIt move(InputIt first, InputIt last, OutputIt d_first)
{
        while (first != last) {
                    *d_first++ = std::move(*first++);
                        }
            return d_first;
}
```

##注意

移动重叠区间时，如果目标区间的起始位置不在源区间内，那么用[std::move](move.md) 移动到目标区间左侧是合适的；如果目标区间的终止位置不在源区间内，那么用[std::move_backward](move_backward.md) 移动到目标区间右侧亦是合适的。

##例子

下面的例子是把本身不可复制的线程对象从一个容器移动到另一个容器。

```C++
#include <iostream>
#include <vector>
#include <list>
#include <iterator>
#include <thread>
#include <chrono>

void f(int n)
{
    std::this_thread::sleep_for(std::chrono::seconds(n));
    std::cout << "thread " << n << " ended" << '\n';
}

int main()
{
    std::vector<std::thread> v;
    v.emplace_back(f, 1);
    v.emplace_back(f, 2);
    v.emplace_back(f, 3);
    std::list<std::thread> l;
    // copy() would not compile, because std::thread is noncopyable

    std::move(v.begin(), v.end(), std::back_inserter(l));
    for (auto& t : l) t.join();
}
```

输出为：

```C++
thread 1 ended
thread 2 ended
thread 3 ended
```

##请参阅

- [move_backward](move_backward.md)（C++11）    逆向移动区间里的元素到一个新的位置（函数模板）
- [move](../utility/move.md)（C++11）           转换为右值引用（函数模板）
