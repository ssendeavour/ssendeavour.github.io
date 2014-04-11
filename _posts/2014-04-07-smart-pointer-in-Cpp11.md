---
layout: post
title: "smart pointer in C++11 (notes)"
date: Mon Apr  7 00:35:15 CST 2014
tags: [C++, C++11]
categories: C++
code: 1
mathjax: 0
---
{% include JB/setup %}

smart pointer in C++11 (notes)
---

---

~~~cpp
auto c = make_shared<circle>(12);
auto ints = make_unique<int[]>(new int[10]);

// customize deleter
// wrap C pointer
auto stmt = make_shared<sqlite3_stmt>(nullptr, [](sqlite3_stmt * stmt){
	sqlite3_free(stmt);
	stmt = nullptr;
});

const char * sql_template = "delete from BOOK where book_id = '%s';";
size_t sql_length = std::strlen(sql_template) + sizeof(T);
std::unique_ptr<char[]> sql(new char[sql_length]);
std::snprintf(sql.get(), sql_length, sql_template, book_id.c_str());
~~~
