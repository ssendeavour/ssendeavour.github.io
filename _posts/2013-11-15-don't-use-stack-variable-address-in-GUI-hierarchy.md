---
layout: post
title:  "经验：GUI编程中不要在栈上分配GUI类对象"
date: Fri Nov 15 20:00:38 CST 2013
tags: [C++, GUI, Qt, Pointer]
categories: C++ 
---
{% include JB/setup %}

各种GUI类库的常用对象，在构建时其内部都会维护着对象之间的父子关系，这也是它们的构造函数都要求传递一个GUI类库最顶层的类的指针的原因。每个对象维护着它的子对象列表，当这个对象析构时，会遍历子对象列表， 删除子对象。这样可以确保父子对象的生命周期同时结束。程序员不需要关心子对象什么时候删除。

但是不要把在栈上建立的对象的地址传递作为parent,传递给构造函数，这样容易出错，析构时会析构再次，而析构函数只能调用一次，因此程序就会异常退出。

刚才我就遇到了这个问题，纠结了两个多小时。我原本需要的是一个pointer to array of 4 pointers of class QTableView 的指针数组，但是由于没有正确的定义这个数组，最后变成了pointer to array of 4 class QTableView. 数组的内容是对象而不是对象指针。接着在建立其他对象时就直接传了这个数组里的对象的地址（用＆取地址，现在想想这是很不应该的，到这一步就应该Google 一下正确的 poniter to array of 4 poiners 怎样写了。

