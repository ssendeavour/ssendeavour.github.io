---
layout: post
title: "Ubuntu: update-alternatives command"
date: Thu Mar 27 22:02:11 CST 2014
tags: [Ubuntu, update-alternatives]
categories: Software
code: 1
mathjax: 0
---
{% include JB/setup %}

update-alternatives 命令
---

如果安装了同一个软件的不同版本，可以使用`update-alternatives`命令设置默认使用哪个版本，典型的如在Ubuntu 12.04里安装了`gcc-4.6`和`gcc-4.8`，想让`gcc`命令自动使用4.8版。

---

安装g++-4.8后，将其设置为默认。gcc同理

~~~bash
# 首先要让系统知道我们安装了多个版本的g++
# 命令最后的 20和50是优先级，如果使用auto选择模式，系统将默认使用优先级高的
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50

# 使用交互方式的命令选择默认使用的版本
sudo update-alternatives --config g++ 

# 其他命令：
#	查询系统中安装有哪些版本
sudo update-alternatives --query g++
~~~

###参考

- `man 8 update-alternatives`


