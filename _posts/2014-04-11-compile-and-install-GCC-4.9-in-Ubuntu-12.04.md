---
layout: post
title: "Compile and install GCC-4.9 in Ubuntu 12.04"
date: Fri Apr 11 13:08:50 CST 2014
tags: [GCC]
categories: Tool
code: 1
mathjax: 0
---
{% include JB/setup %}

在Ubuntu 12.04下编译安装GCC/G++4.9
---

做毕设使用的一个开源项目restbed使用`g++4.9`才能编译，4.9还没正式发布，Ubuntu 12.04没有预编译好的，无奈只好自己编译。

---

1. 下载最新的开发版GCC源代码

~~~bash
svn checkout svn://gcc.gnu.org/svn/gcc/trunk
~~~

2. 安装依赖

~~~
sudo apt-get install flex bison build-essential 

# 如果要支持multilib（即使编译出来的gcc可以编译32位和64位的程序），需要安装下面的依赖
sudo apt-get install gcc-multilib
#如果你现用的gcc编译器不是Ubuntu 12.04源里的4.6版本，比如我使用的是ppa里的4.8版本，上面的命令需要改成
sudo apt-get install gcc-4.8-multilib

# 在gcc源代码的根目录执行下面的命令，这会下载gmp, mpfr, mpc，并装它们的编译集成到GCC的编译过程中
./contrib/download_prerequisites 
~~~

3. 建立一个编译用的目录，在gcc源码的根目录下执行这个命令
~~~
mkdir build
cd build
~~~

3. 配置。假设想把这个gcc安装到 `/opt/gcc_4_9/`目录，示例配置如下，使用这个配置，编译出来的gcc支持编译`C`, `C++`两种语言。支持编译32位和64位程序
~~~bash
../configure --prefix=/opt/gcc_4_9 --program-suffix=-4.9 --enable-languages=c,c++  --enable-multilib \
	--build=x86_64-linux-gnu --enable-checking=release 
~~~

4. 开始编译，`-j4`表示使用4个线程。在我的电脑上（intel i3）用了50多分钟。

~~~bash
make -j4
~~~

5. 安装与验证

~~~bash
make install

/opt/gcc_4_9/bin/g++-4.9 --version
~~~

-----------
我花了大半天时间才弄好。开始我根据网上的中文教程，自己下载编译gmp, mpfr, mpc。make的时候总是出错。后来再搜才搜到使用 download_prerequisites 的方法安装依赖。但是 flex 和 bison 的依赖还是要装的。
