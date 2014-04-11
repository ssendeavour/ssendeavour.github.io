---
layout: post
title: "Compile and install RESTBED in Ubuntu 12.04"
date: Fri Apr 11 14:39:04 CST 2014
tags: [restbed]
categories: Tool
code: 1
mathjax: 0
---
{% include JB/setup %}

在Ubuntu 12.04 下编译安装restbed
---

restbed 是一个支持RESTful的C++库。编译它需要使用`cmake` >= 2.8.10，和 `g++` >= 4.9。这两个条件在Ubuntu 12.04里都不满足。cmake和gcc都需要自己编译最新的。cmake比较容易编译，gcc4.9的编译请看我前一篇文章。它还依赖`asio`库，这个库Ubuntu 12.04的源里有，直接安装就行了。

---

我想把restbed安装到系统的`/usr/`目录里，这样编写的程序可以比较方便的使用它。所以在编译之前需要修改build/CMakeList.txt

修改 `build/CMakeLists.txt` 文件

1. 第21行改为 `set( CMAKE_INSTALL_PREFIX "/usr" )`
2. 第22行改为 `set( LIBRARY_OUTPUT_PATH    "${CMAKE_INSTALL_PREFIX}/lib" )`
3. 第60行改为 `file( MAKE_DIRECTORY "${CMAKE_INSTALL_PREFIX}/lib" )`

改完后进入`build`子目录，编译安装

~~~bash
sudo cmake . -DCMAKE_CXX_COMPILER=/opt/gcc_4_9/bin/g++-4.9
sudo make clean install
~~~

现在就安装好了。使用时包含头文件`<restbed>`，使用gcc编译时在最后加上编译选项`-l restbed`就OK了。
