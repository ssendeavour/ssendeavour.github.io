---
layout: post
title:  "Modern C++ (MSDN): Exception"
date: Thu Nov 28 13:34:37 CST 2013
tags: [C++, Modern C++, MSDN]
categories: C++
---
{% include JB/setup %}

微软MSDN现代C++：异常exception
---

---

## 错误和异常 errors and exception handling

Errors and Exception Handling (Modern C++)

<http://msdn.microsoft.com/en-us/library/vstudio/hh279678.aspx>

**推荐使用异常**而不是C风格的错误处理（通过检测返回值或者一个全局变量获知错误发生）

 - 头文件： `<stdexcept>`
 - 可抛出任意类型，但推荐抛出的异常类型继承自`std::exception`
 - 以值的方式抛出异常，以引用的方式捕获异常
 
**异常与性能**

 - 如果没有抛出异常，性能损失非常小
 - 如果抛出异常，栈遍历和展开的损失大致相当于一个函数调用
 - 进入`try` 块时需要额外的数据结构记录调用栈
 - 如果抛出异常，需要额外的指令用于展开栈

大多数情况下，性能和内存上的损失并不显著(significant)。

### exception specification and noexcept

exception specification 已经过时，不推荐使用，除非是`throw()`，C++11引入了`noexcept`关键字，推荐用它替代`throw()`。

---

## 异常安全 How to: Design for Exception Safety

<http://msdn.microsoft.com/en-us/library/vstudio/hh279653.aspx>

**摘要**

 - 底层模块应检测并抛出大部分异常，通常这一层没有足够的信息处理异常或者向最终用户发出消息
 - 中间层大多数情况下直接将异常传递到上层。
 - 即使在最上层，如果异常使程序进入一种正确性无法保证的状态，让未处理的异常终止程序也是合理的。

exception-safe的设计原则：

**基本技术：**

 - 保持资源类简单（见原文的例子，使用`shared_ptr`防止构造函数中的异常导致内存泄漏）
 - 使用RAII管理资源（没有例子没看懂，大概就是使用RAII对象（比如智能指针）管理资源，出了作用域后自动调用析构函数释放资源）。

**三种异常担保**

 - **no-fail**(或者**no-throw**)担保是函数能提供的最强的担保。它表示函数不会抛出异常或者允许异常传播。只有满足以下条件才能可靠的提供这一担保：（1）这个函数调用的所有函数也是`no-fail`；或者（2）所有异常在到达这个函数之前都被捕获了；或者（3）这个函数捕获并处理所有可能到达的异常。
 
	strong 担保和 basic 担保都假设析构函数是`no-fail`的。标准库中的所有容器和类型保证它们的析构函数不会抛出异常。标准库要求用户提供的自定义定型（如模板参数）的析构函数必需不抛出异常。

- **strong**担保：函数由于异常离开作用域时，它不会泄漏内存，程序状态不会被修改。提供这个担保的函数实质上是一种提交(commit)或者回滚(rollback)的语义————要么成功，要么没有效果。

- **basic**担保：是三种担保中最弱的，但却可能是当strong担保在性能或者内存消耗上的代价太昂贵时最合适的选择。basic担保是说，当异常发生时，内存不会泄漏，并且对象仍然在可用状态，尽管数据可能被修改了。

**异常安全的类**

如果类的构造函数在完成之前退出，那么这个对象并没有创建，它的析构函数也不会调用，动态分配且没有使用智能指针或者类似自动变量(automatic variable)管理的内存将会泄漏。

指导原则：

 - 使用智能指针或者其他RAII包裹类管理所有资源。避免在类的析构函数中管理资源，因为有异常抛出时析构函数将得不到调用。例外情况：如果类是专门用于管理一种资源的管理器，那么在析构函数中管理资源是可行的。
 - 基类构造函数中抛出的异常无法在派生类构造函数中消化掉(swallowed)，如果要在派生类构造函数中解释（translate）并重新抛出基类的异常，需要使用函数try块，参考How to: Handle Exceptions in Base Class Constructors (C++).(<http://msdn.microsoft.com/en-us/library/vstudio/e9etx778.aspx>)。
 - 考虑是否（需要）把类的所有数据成员保存在智能指针里，特别是类的构造函数允许失败的时候。
 - 析构函数中不能抛出异常。如果有异常，必需捕获并且消化（swallow）掉。标准库定义的所有析构函数提供这种保证。

---

## 异常与栈展开 Exceptions and Stack Unwinding in C++

<http://msdn.microsoft.com/en-us/library/vstudio/hh254939.aspx>

**摘要：**

 * 如果在栈展开的过程中，异常处理程序获得控制权之前（又）发生异常，运行时函数`std::terminate`将被调用。
 * 如果在一个异常被抛出之后、栈展开开始之前（又）发生异常，将调用`std::terminate`。
 * 如果找到了匹配的异常处理程序，根据处理程序按值还是按引用捕获异常，它的formal parameter（指catch后面括号里的参数）将被相应的初始化。
 * formal parameter 初始化完成后，开始进行栈开展。与`catch`对应的`try`到异常抛出现场之间已经构造完成的自动变量（对象）将会被析构。析构顺序与构造顺序相反。
 * `catch`块只能通过抛出异常进入，`goto`或者`switch`中的`case`标签则不能。
 * 发生异常时，异常发生现场和try之间的函数调用将不会返回，如果在这之间有用`new`在堆上分配的内存，异常发生现场与`catch`之间的释放内存的`delete`操作将不会执行。

**更多资料**

 - C++ Exception Handling: <http://msdn.microsoft.com/en-us/library/vstudio/4t3saedz(v=vs.120).aspx>

