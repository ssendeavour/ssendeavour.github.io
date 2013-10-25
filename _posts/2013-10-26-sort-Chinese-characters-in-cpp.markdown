---
layout: post
title:  "sort Chinese characters using c++ | C++按照拼音对中文排序"
date:   Sat Oct 26 00:42:24 CST 2013
categories: cpp
---
方法来自《C++ cookbook》，书中以德语为例，我的工作是解决了在Ubuntu中locale名称不对的问题。locale名称的查找方法：

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

bool localeLessThan(const string &s1, const string &s2){
	const collate<char>& col = 
		use_facet<collate<char> >(locale());		// use the global locale
	const char *pb1 = s1.data();
	const char *pb2 = s2.data();
	return (col.compare(pb1, pb1+s1.size(), pb2, pb2+s2.size()) < 0);
}

int main(void){
	// need -std=c++0x flag
	vector<string> v = {"啊", "阿", "第一", "第二", "第贰", "第叁", "第三",
		"si", "shi", "wu", "w", "第六", "六" };

	// set the golbal locale to Chinese, print old locale
	try{
		cout << "original locale: " << locale::global(locale("zh_CN.utf8")).name() << endl;
	} catch (exception& e){
		cerr << e.what() << endl;
		return EXIT_FAILURE;
	}

	cout << "current locale: " << locale().name() << endl;
	sort(v.begin(), v.end(), localeLessThan);

	// restore C locale
	locale::global(locale::classic());
	cout << "restored locale: " << locale().name() << endl;

	for(vector<string>::const_iterator p = v.begin(); p != v.end(); ++p){
		cout << *p << endl;
	}

	return EXIT_SUCCESS;
}
{% endhighlight %}

编译运行结果：

{% highlight text %}
➜  i18n  g++ -Wall -std=c++0x sort_chinese_characters.cpp
➜  i18n  ./a.out 
original locale: C
current locale: zh_CN.utf8
restored locale: C
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
