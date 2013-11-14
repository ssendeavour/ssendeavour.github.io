---
layout: post
title: "C++ typecasting: dynamic_cast, reinterprete_cast, static_cast, const_cast, typeid"
tagline: "C++"
description: ""
tags: [C++, typecast, dynamic_cast, reinterprete_cast, static_cast, const_cast, typeid]
---
{% include JB/setup %}

本文是cplusplus.com上文章的笔记，原文：
<http://www.cplusplus.com/doc/tutorial/typecasting/>

{:toc}

---

## 传统类型转换

### 隐式类型转换(implicit conversion)

隐式类型转换自动进行，不需要程序员关心（确切的说是不需要程序员写代码，不关心可是不行的:-)。例如：

~~~cpp
short a = 2000;
int b = a;
~~~
上例中，`a`的值自动从`short`提升为`int`，然后赋给`b`。这种转换常见于数值类型，可以从低精度到高精度，自动进行转换。

隐式类型转换也包括类的单参数构造函数和运行符重载的转换。例如：

~~~cpp
class A{};
class B{ public: B(A a){} };
A a;
B = a;
~~~
在上例中，类`B`含有参数类型为`A`的构造函数，所以从`A`到`B`的隐式类型转换是合法的，这通常也是需要特别注意的:)

---

### 显式类型转换

直接看例子：

~~~cpp
short a = 2000;
int b;
b = (int) a;	// C 风格
b = int (a);	//函数调用风格
~~~

上面这种类型转换对基础数据类型已经足够，但是这些转换也可以不加区分的应用于类和指向类对象的指针。这就容易写出语法正确，运行时出错的代码：

~~~cpp
class A{ ... };
class B{ ... };

int main(){
	A a;
	B *b = (A*)&a; //运行时错误
}
~~~

---

##C++新引入的类型转换

### dynamic_cast：派生类 --> 基类

`dynamic_cast` 只能用于指针或者引用类型, 它确保转换的结果是有效完整的要转换成的类的对象。

使用情景：把一个类对象转换为它的基类对象。Derived --> Base

~~~cpp
class Base {...};
class Derived:public Base{ ... };
Base b, *pb;
Derived d, *pd;
pb = dynamic_cast<Base *>(&d);		//派生类 --> 基类，没有问题
pd = dynamic_cast<Derived *>(&b);	//基类 --> 派生类，错误
~~~

第二个转换是错误的，基类-->派生类的转换只有在**基类是多态**的情况下才有效。

---

当一个类是多态类时，`dynamic_cast`进行运行时检查，确保转换出来的对象是有效完整的要转换成的类的对象(valid complete object of the requested class)。

~~~cpp
#include <iostream>
#include <exception>

class Base {public: virtual void dummy() {} };
class Derived: public Base { int a; };

int main(){
	try{
		Base *pba = new Derived;
		Base *pbb = new Base;
		Base b;
		Base &rb = b;

		Derived *pd = dynamic_cast<Derived *>(pba);
		pd == nullptr && std::cout << "pd is nul" << std::endl;
		Derived *pd2 = dynamic_cast<Derived *>(pbb);
		pd2 == nullptr && std::cout << "pd2 is nul" << std::endl;
		void *pv = dynamic_cast<void *>(pba);
		pv == nullptr && std::cout << "pv is nul" << std::endl;
		Derived &rd = dynamic_cast<Derived &>(rb);
	} catch (std::exception &e) {
		std::cout << "Exception: " << e.what() << std::endl; }
	return 0;
}
~~~

运行结果：

~~~shell
pd2 is nul
Exception: std::bad_cast
~~~

上例中第二个转换不能产生完整的对象，因此`dynamic_cast`返回`nullptr`表示转换失败。如果`dynamic_cast`用于转换引用类型，转换失败时会抛出`bad_cast`类型的异常。

`dynamic_cast`也可用于把任意类型的指针转为空指针（`void *`）。

---

### static_cast：派生类-->基类、基类-->派生类、C风格转换

可进行派生类 <--> 基类的互相转换，它不像`dynamic_cast`那样在运行时检查转换结果的有效性和完整性。靠程序员来确保转换的安全性。

~~~cpp
class Base {};
class Derived: public Base {};
Base *b = new Base;
Derived *d = static_cast(<Derived *>(a);
~~~

上例的转换是有效的，虽然这样`b`会指向一个不完整的类对象并且解引用(deferenced）时会导致运行时错误。

`static_cast`也可用于任何可以隐式进行的非指针的转换，例如：

~~~cpp
double d = 3.141592653;
int i = static_cast<int>(d);
~~~

---

### reinterpret_cast：任意类型 --> 任意类型

`reinterpret_cast`把任意类型转为其他任意类型，即使是不相关的类型。这个操作的结果只是简单的把值从一个指针复制到另一个。任何指针转换都可以：既不检查指向的内容，也不检查指针自身的类型。

从名字就可以看出来这一点：`reinterprete`，重新解释内存中的数据，也就不管这些数据原来是什么类型了，不要忘了程序数据（类对象，基础类型）只不过是内存中的二进制流。4字节的int类型不过是连续的32个位(bit)，我完全可以把它当作UTF-32编码的一个Unicode文字，虽然这个文字与前后的内容放在一起时，很可能没有意义，所以我们通常不会这样做，但我完全**可以**这样做，贵在**可以**。`reinterprete`就是用来干这个事的。

**指针 <--> 整型**：整型的表示方式与具体平台相关。唯一能够保证的是：把指针转换为一个能容纳它的整型，这个整型就可以再转成一个有效的指针。

能够使用`reinterpret_cast`而不能使用`static_cast`进行的转换是低层的操作，它的解释通常与与系统相关，是不可移植的。

---

### const_cast：去掉const属性

原文说的是`const_cast`操作对象的`constness`，或者加上，或者去除。但是我不知道怎样能加上，非const的变量可以作为参数传递给需要`const`参数的函数，而使用`const_cast<const char *>(s)`或者`const_cast<char *>(s)`试图把非`const`的`s`加上`const`属性时，g++没有报错。但是转换与不转换，g++给出的警告是一样的：

>warning: deprecated conversion from string constant to ‘char*’ [-Wwrite-strings]

所以，`const_cast`能否用于“加上”`const`属性就存在疑问了。

例如：为了把一个`const`参数传给一个需要非`const`参数的函数，需要去掉(cast away)`const`：

~~~cpp
#include <iostream>
using namespace std;

void print(const char * str){
	cout << str << endl;
}
int main(){
	const char * s = "sample";
	print(const_cast<char *>(s));
	return 0;
}
~~~

---

### typeid()：取得表达式的类型

这个操作符返回一个`type_info`类型的常量对象的引用。返回值可以使用`==`，`!=`与其他值进行比较，也可以使用`name()`成员得到一个以`null`结尾的字符串代表数据类型或者类名。

~~~cpp
#include <iostream>
#include <typeinfo>
using namespace std;

class Foo {public: virtual void f() {} };
class Bar: public Foo {};

int main(){
	int *a, b;
	a = 0; b = 0;
	if(typeid(a) != typeid(b)){
		cout << "a and b are different types" << endl;
		cout << "a is: " << typeid(a).name() << endl;
		cout << "b is: " << typeid(b).name() << endl;
	}
	cout << typeid(int).name() << endl;
	cout << typeid(double).name() << endl;
	Foo f;
	cout << typeid(f).name() << endl;
	cout << typeid(Foo).name() << endl;

	Foo *pf = new Bar;
	cout << typeid(pf).name() << endl;
	cout << typeid(*pf).name() << endl;

	cout << typeid(cout).name() << endl;
	cout << typeid(cin).name() << endl;
	cout << typeid(cerr).name() << endl;
	return 0;
}
~~~

运行结果(Ubuntu 12.04.3, g++ 4.6.4)

~~~shell
a and b are different types
a is: Pi
b is: i
i
d
3Foo
3Foo
P3Foo
3Bar
So
Si
So
~~~

注：`name()`返回的字符串与编译器实现有关，可以是任何字符串。

如果`typeid`应用于多态类，返回的结果是“the type of the most derived complete object”(不知道怎么翻，也不懂意思）。

<del>如果`typeid`的参数是解引用的指针(*p)，并且指针为`nullptr`，将抛出`bad_typeid`异常。</del>在我的试验里，没有抛出异常，与指针不为空时行为一致。


