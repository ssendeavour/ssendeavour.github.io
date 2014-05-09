---
layout: post
title: "Hidden Features and Dark Corners of C++/STL"
date: Mon Apr 21 14:27:16 CST 2014
tags: [C++]
categories: C++
code: 1
mathjax: 0
---
{% include JB/setup %}

---

来自：[Google groups comp.lang.c++](https://groups.google.com/forum/#!msg/comp.lang.c++.moderated/VRhp2vEaheU/IN1YDXhz8TMJ)

---

1. when a temporary is assigned to a reference inside a {} block, it's lifetime is extended to the lifetime of a reference.

~~~cpp
#include <iostream>
using namespace std;

int main()
{
	int a = 10;
	int & ra = a;
	{
		int t = 12;
		ra = t;
	}

	cout << ra << endl;
}
~~~
