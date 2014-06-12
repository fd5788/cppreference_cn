# 模板（Templates）

C++ 的模板是这样一个独立部分（entity），它至少定义如下之一：

- 类模板（[class template](class_template)），可以嵌套（[nested classes](class_member_templates)）
- 函数模板（[function template](function_template)），可以作为成员函数（[function member](class_member_templates)）出现
- 别名模板（[alias template](../declarations/type_alias)）（C++11起）
- 变量模板（[variable template](?)）（C++14起）

模板可以被一个或多个模板形参（[template parameters](template_parameters)）参数化。模板形参有三种类型，类型模板形参（type template parameters）、非类型模板形参（non-type template parameters）和模板型模板形参（template template parameters）。

要特化一个模板，需要提供模板实参（template arguments）或者，专门针对函数模板的[实参推演](function_template#template_argument_deduction)，以此替代模板形参。模板实参可以是特定的类型或者特定的函数左值。当然，也可以显式特化模板：全特化，支持类模板和函数模板；部分特化，只支持类模板。

在需要完全对象类型或函数定义存在的上下文中，模板将在引用类模板特化或函数模板特化时实例化，除非该模板已被显式特化或是显式实例化。类模板实例化只会实例化需要用到的成员函数，而不是实例化所有的成员函数。链接时，不同编译单元生成的等同的模板实例将被合并。

隐式实例化模板时该模板的定义必须已知，这就是为什么模板库通常在头文件中定义模板。
