---
layout: post
title:  "sort Chinese characters using c++ | C++按照拼音对中文排序"
date:   Sat Oct 26 00:42:24 CST 2013
categories: cpp
---
方法来自《C++ cookbook》，书中以德语为例，我的工作是解决了在Ubuntu中locale名称不对的问题。调用sort函数前不用再改变全局的locale，这里没有处理locale名称找不到时抛出的`runtime_error`。

locale名称的查找方法：

- `man 7 locale`，我是从这里找到的。cookbook上用的是`locale("french")`, `locale("english-american")`, 作者用的编译器是`VC++7.1`。我试过`locale("chinese")`, `locale("chinese-china")`都不行。
- [libstdc++ 文档](http://gcc.gnu.org/onlinedocs/libstdc++/latest-doxygen/a01285.html)

C++标准只规定了一个locale：`C`。其他locale可能不是在所有平台上都可用。我的环境：Ubuntu 12.04.3, g++ 4.6.3, $LANG=en_US.UTF-8


{% highlight cpp %}
#include <iostream>
#include <string>
#include <locale>
#include <vector>
#include <algorithm>

using namespace std;

static const locale zh_CN_locale = locale("zh_CN.utf8");
static const collate<char>& zh_CN_collate = use_facet<collate<char> >(zh_CN_locale);

bool zh_CN_less_than(const string &s1, const string &s2){
	const char *pb1 = s1.data();
	const char *pb2 = s2.data();
	return (zh_CN_collate.compare(pb1, pb1+s1.size(), pb2, pb2+s2.size()) < 0);
}

int main(void){
	// need -std=c++0x flag
	vector<string> v = {"啊", "阿", "第一", "第二", "第贰", "第叁", "第三",
		"si", "shi", "wu", "w", "第六", "六" };

	sort(v.begin(), v.end(), zh_CN_less_than);

	for(vector<string>::const_iterator p = v.begin(); p != v.end(); ++p){
		cout << *p << endl;
	}

	return EXIT_SUCCESS;
}
{% endhighlight %}

编译运行结果：

{% highlight text %}
➜  i18n  g++ -std=c++0x sort_chinese_characters.no_global_locale.cpp 
➜  i18n  ./a.out 
shi
si
w
wu
阿
啊
第二
第贰
第六
第三
第叁
第一
六
➜  i18n 
{% endhighlight %}
