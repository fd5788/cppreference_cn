#算法库（Algorithm library）

算法库定义了各种各样功能的操作于区间的函数（如，搜索，排序，计数，操纵等）。注意区间定义为 [first, last)，其中 `last`指向待检查或修改的最后一个元素的下一个位置。

##非修改顺序操作

定义于头文件[algorithm](algorithm.md)中：

---
[all_of](all_any_none_of.md)  
[any_of](all_any_none_of.md)  
[none_of](all_any_none_of.md)

判断对于区间里的所有元素、任一元素或没有元素，某断言是否是为真（函数模板）

---
[for_each](for_each.md)

某函数作用于区间里的每一个元素（函数模板）

---
[count](count.md)   
[count_if](count.md)

返回满足特定准则的元素的个数（函数模板）

---
[mismatch](mismatch.md) 

找出两个区间相异的第一个位置（函数模板）

---
[equal](equal.md)（函数模板）

判断两个集合的元素是否一样

---
[find](find.md)  
[find_if](find.md)  
[find_if_not](find.md)

找出满足特定规则的第一个元素（函数模板）

---
[find_end](find_end.md)

找出区间里最后一个匹配的子序列(函数模板）

---
[find_first_of](find_first_of)

搜索区里出现在另一个区间的任何元素（函数模板）






