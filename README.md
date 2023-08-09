# cs106l-hashmap
----
CS 106L, Standard C++ Programming, Stanford University, Spring 2023.

不知道如何实现移动构造函数和移动赋值函数的时候可以看看 cppreference，参考手册给了一个很好的示例。

## 关于右值引用
困扰我很久的右值引用，实际上右值引用并不是延长对象的生命周期，只不过标记这个变量即将销毁，它的资源可以被转移。

- Only l-values can be referenced using &.
- rvalues can be bound to `const &` (we promise not to change them)

因此接收一个引用作为参数的函数，这个参数不能是字面量（字面量是右值）。`const &`可以接收左值也能接收右值，但是把`const &`的参数赋值给其他变量时不能避免拷贝。

下面是从winter 2020的课件摘抄

- l-value is not safe to move copy instead.
- r-value is safe to move.
- Compiler prefers T&& for rvalues if implemented

个人理解：区分出 r-value 就是区分出可以 move 的值，来避免拷贝。声明为 r-value 或者使用 std::move() 就是说这个变量所拥有的资源是可以放心剽窃（steal）的，因为 r-value 是马上就要析构了的。

> An __r-value reference__ is an alias to an __r-value__
>
> **BUT** the _r-value reference itself_ is an _l-value_

刚刚接触到右值引用的的时候，不知道该什么时候用，也不知道到底怎么move。对于右值引用的变量就把它当作一个变量就行了，要想这个变量赋值给其他变量的时候调用移动构造函数就把这个变量转换为右值（调用 std::move）。

## std::move
- std::move(x) __doesn’t do anything__ except cast x as an r-value
- It is a way to force C++ to choose the && version of a function

std::move 没有做什么移动，移动发生在赋值的时候。

## 笔记
3.
C++, By default, makes copies when we do variable assignment! We need to use & if we need references instead.

Fundamental Theorem of Software Engineering: Any problem can be solved by adding enough layer of indirection.

The Takeaway
Templates don't emit code until instantiated so include the .cpp in the .h instead of the other way around!

模板直到实例化的时候才会生成代码，所以模板类的实现应该要写在 .h 里面或者在 .h 里面 include .cpp 文件。
