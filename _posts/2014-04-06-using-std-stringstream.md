---
layout: post
title: "using std::stringstream"
date: Sun Apr  6 16:16:20 CST 2014
tags: [C++, stringstream, io]
categories: C++
code: 1
mathjax: 0
---
{% include JB/setup %}

一个`stringstream`的对象`ss`，当它的状态是`eof()`，就会一直在这个状态不能自拔，即使使用`ss.str(new_string)`也没用。所以在再次使用`<<`, `>>`之前要重置它的状态。
---

---

~~~cpp
std::string line = "word1 word2 word3";
std::stringstream ss(line);
std::string word;

while(ss >> word){
	std::cout << word << std::endl;
}

line = "some other sentence";

// ss.str(line) 是将line的内容附加到ss原来的字符串后面，而不是替换它的内容。
ss.str("");	 // 先清空ss里可能剩余的字符
ss.str(line); 

// 恢复 ss 流的状态，上一步读结束后，它的状态很可能是EOF，
// 若不清空，接下来的读都不会成功
ss.clear();

while(ss >> word){
	std::cout << word << std::endl;
}
~~~

如果正确的使用 `stringstream`, `iostream` 等输入输出流，请参考`C++ FAQ`，它提供了不少示例代码。

<http://isocpp.org/wiki/faq/input-output>

