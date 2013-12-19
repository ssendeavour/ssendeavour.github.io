---
layout: post
title: "一段C语言代码——变量作用域与隐藏"
date: Fri Dec 20 01:36:07 CST 2013
tags: [C, snippet]
categories: C/C++
code: 1
mathjax: 0
---
{% include JB/setup %}


一段C语言代码，新手可用来练习各种概念。对里面的东西有些我也不太确定，但真正的项目可能不会遇到这么复杂的情况，所以也不打算深究（深究的产出和成本不成比例）

~~~c
#include <stdio.h>

typedef struct x {
    int x;
} x;

x ax;

#define x(a) (a)

int main()
{
    int x = (int)(&ax);
x:    ((struct x *)x)->x = x(5);
    printf("%d\n", ax);
}
~~~

来源： <http://bbs.stuhome.net/forum.php?mod=viewthread&tid=1396935>
