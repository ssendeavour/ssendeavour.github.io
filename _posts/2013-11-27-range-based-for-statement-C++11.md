---
layout: post
title:  "Range-based for statement - C++11"
date: Wed Nov 27 20:01:42 CST 2013
tags: [C++, C++11]
categories: C++
---
{% include JB/setup %}

C++11 基于范围的 for 语句推荐用法
----------

---

来自[MSDN (http://msdn.microsoft.com/en-us/library/vstudio/jj203382.aspx)](http://msdn.microsoft.com/en-us/library/vstudio/jj203382.aspx)

示例：

~~~cpp
// range-based-for.cpp
#include <iostream>
#include <vector>
using namespace std;

int main() 
{
    // Basic 10-element integer array.
    int x[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Range-based for loop to iterate through the array.
	// ----不推荐，需要复制 x ----
    for( int y : x ) { // Access by value using a copy declared as a specific type. 
                       // Not preferred.
        cout << y << " ";
    }
    cout << endl;

    // The auto keyword causes type inference to be used. Preferred.
	// ----不推荐, 需要复制 x ----
    for( auto y : x ) { // Copy of 'x', almost always undesirable
        cout << y << " ";
    }
    cout << endl;

	// ----推荐, 需要进行修改时使用----
    for( auto &y : x ) { // Type inference by reference.
        // Observes and/or modifies in-place. Preferred when modify is needed.
        cout << y << " ";
    }
    cout << endl;

	// ----推荐, 不需要进行修改时使用----
    for( const auto &y : x ) { // Type inference by reference.
        // Observes in-place. Preferred when no modify is needed.
        cout << y << " ";
    }
}
~~~

推荐的写法：

- 使用 `auto` 关键字, 使用引用
- 如果需要修改元素，使用不带`const`关键字的引用(`for( auto &x : y)`)。否则使用带 `const`的引用(`for(const auto &x : y)`)。
