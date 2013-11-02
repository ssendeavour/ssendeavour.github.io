---
layout: post
title:  "sort Chinese characters with C++ | C++按照拼音对中文排序"
date:   Sat Oct 26 00:42:24 CST 2013
categories: cpp
---
方法来自《C++ cookbook》，书中以德语为例，思想是设置程序使用的locale为相应语言，按照语言的locale进行排序。

我的工作是解决了在Ubuntu中locale名称不对的问题。还有调用sort函数前不用再改变全局的locale，这里没有处理locale名称找不到时抛出的`runtime_error`。

locale名称的查找方法：

- `man 7 locale`，我是从这里找到的。cookbook上用的是`locale("french")`, `locale("english-american")`, 作者用的编译器是`VC++7.1`。我试过`locale("chinese")`, `locale("chinese-china")`都不行。
- [libstdc++ 文档](http://gcc.gnu.org/onlinedocs/libstdc++/latest-doxygen/a01285.html)

C++标准只规定了一个locale：`C`。其他locale与实现有关，名称也不尽相同。

	#include <iostream>
	#include <string>
	#include <locale>
	#include <vector>
	#include <algorithm>

	using namespace std;

	// Linux g++ locale 名称: "zh_CN.utf"
	// VC2010 locale 名称：	"Chinese"或者"Chinese_china"
	#ifdef _MSC_VER
	static const char *ZH_CN_LOCALE_STRING = "Chinese_china";
	#else
	static const char *ZH_CN_LOCALE_STRING = "zh_CN.utf8";
	#endif

	static const locale zh_CN_locale = locale(ZH_CN_LOCALE_STRING);
	static const collate<char>& zh_CN_collate = use_facet<collate<char> >(zh_CN_locale);

	bool zh_CN_less_than(const string &s1, const string &s2){
		const char *pb1 = s1.data();
		const char *pb2 = s2.data();
		return (zh_CN_collate.compare(pb1, pb1+s1.size(), pb2, pb2+s2.size()) < 0);
	}

	int main(void){
		vector<string> v;
		v.push_back("啊");
		v.push_back("阿");
		v.push_back("第一");
		v.push_back("第二");
		v.push_back("第贰");
		v.push_back("di");
		v.push_back("第三");
		v.push_back("liu");
		v.push_back("第叁");
		v.push_back("第四");
		v.push_back("abc");
		v.push_back("aa");

		cout << "locale name: " << zh_CN_locale.name()<< endl;
		sort(v.begin(), v.end(), zh_CN_less_than);
		for(vector<string>::const_iterator p = v.begin(); p != v.end(); ++p){
			cout << *p << endl;
		}

		return EXIT_SUCCESS;
	}

在Linux 上使用 g++ 编译，我的环境：

~~~
系统：Ubuntu 12.04.3 64bit %
g++: 4.6.3
LANG环境变量：zh_CN.UTF8
~~~

运行结果：

~~~
➜  i18n  g++ sort_chinese_characters.cpp        
➜  i18n  ./a.out 
locale name: zh_CN.utf8
aa
abc
di
liu
阿
啊
第二
第贰
第三
第叁
第四
第一
➜  i18n  
~~~

在英文Windows 7 环境下的 Visual Studio 2010上运行结果：

~~~
locale name: Chinese (Simplified)_People's Republic of China.936
aa
abc
di
liu
阿
啊
第二
第贰
第三
第叁
第四
第一
Press any key to continue . . .
~~~
