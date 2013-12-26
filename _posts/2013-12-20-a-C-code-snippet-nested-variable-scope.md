---
layout: post
title: "一段C语言代码——变量作用域与隐藏"
date: Fri Dec 20 01:36:07 CST 2013
tags: [C, code snippet]
categories: C/C++
code: 1
mathjax: 0
---
{% include JB/setup %}


一段C语言代码，新手可用来练习各种概念。对里面的东西有些我也不太确定，但真正的项目可能不会遇到这么复杂的情况，所以也不打算深究

~~~c
#include <stdio.h>

/* 3. 4. struct/union/enum 中的tag或者元素  */
typedef struct x {
    int x;
} x;

x ax;

/* 1. 预处理器的宏名称  */
#define x(a) (a)

int main()
{
	/* 5. 其他类型 */
    int x = (int)(&ax);

/* 2. goto 语句的label */
x:    ((struct x *)x)->x = x(5);
    printf("%d\n", ax);
}
~~~

来源： <http://bbs.stuhome.net/forum.php?mod=viewthread&tid=1396935>


==============

**Update更新**(2013-12-26)

看了这里 [c reference manual读书笔记(三)](http://www.pagefault.info/?p=319)，提到C允许5种类型的重载

> C语言中允许5种类型的overload，overload是指相同名字的标示符。他们分别是 预处理器的宏名字，goto语句中的label，结构体/union/enum中的tag或者元素,最后剩下的归于一类.

我已经按我的理解在上面的代码中把这五种类型标出来了。

