---
layout: post
title: "smart pointer in C++11 (notes)"
date: Sat Apr  5 15:12:55 CST 2014
tags: [C++, C++11]
categories: C++
code: 1
mathjax: 0
---
{% include JB/setup %}

smart pointer in C++11 (notes)
---

---

~~~C++
auto c = make_shared<circle>(12);
auto ints = make_unique<int[]>(new int[10]);

// customize deleter
// wrap C pointer
auto stmt = make_shared<sqlite3_stmt>(nullptr, [](sqlite3_stmt * stmt){
	sqlite3_free(stmt);
	stmt = nullptr;
});

~~~
