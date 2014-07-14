#稳定排序（std::stable_sort）

定义于头文件`<algorithm>`中：

```C++
template< typename RandomIt >
void stable_sort( RandomIt first, RandomIt last );    (1)
```
```C++
template< typename randomIt, typename Compare >
void stable_sort( RandomIt first, RandomIt last, Compare comp );       (2)
```

升序排序区间`[first, last)`里的元素，注意不包括`last`所指向的元素，*同时保证相等元素的先后顺序不变*，即**稳定排序**。版本(1)用`<`排序，即默认为升序，版本(2)用自定义的函数`cmp`。

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

O(N*log^2(N)), 其中`N = std::distance(first, last)`为比较次数。若能够使用额外空间，则复杂度会降为O(N*log(N))。

##注意

该函数会先尝试分配等同于待排序列长度大小的临时缓冲区，通常通过调用库函数`std::get_temporary_buffer`来实现。如果空间分配失败，将会选择低效的版本。

##例子

```C++
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

struct Employee {
    Employee(int age, std::string name) : age(age), name(name) { }
    int age;
    std::string name;  // Does not particpate in comparisons
};

bool operator<(const Employee &lhs, const Employee &rhs) {
    return lhs.age < rhs.age;
}

int main()
{
    std::vector<Employee> v = {
        Employee(108, "Zaphod"),
        Employee(32, "Arthur"),
        Employee(108, "Ford"),
    };

    std::stable_sort(v.begin(), v.end());

    for (const Employee &e : v) {
        std::cout << e.age << ", " << e.name << '\n';
    }
}
```

输出为：

```C++
32, Arthur
108, Zaphod
108, Ford
```

##另见

- [partial_sort](partial_sort.md)   按一定顺序排出区间里的前N个元素
- [sort](stable_sort.md)     递增排序，且不一定会保持相等元素的先后次序
