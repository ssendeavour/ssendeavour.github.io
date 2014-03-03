---
layout: post
title: "在Ubuntu12.04里编译fcitx-qt5"
date: Fri Nov 29 03:49:29 CST 2013
tags: [Ubuntu, qt5, fcitx, 五笔]
categories: Software, Software & Tool
---
{% include JB/setup %}

fcitx无法在Qt5的窗口里输入中文，需要安装fcitx-qt5包，Ubuntu源里有其他版本的包，唯独没有12.04的。只好自己编译了。前提是安装了Qt5的开发工具（这应该不是必需的，但是我不熟悉cmake，不知道怎样把依赖的文件放在源代码目录下）。

1.	git clone代码 

	~~~shell
	git clone git://anonscm.debian.org/pkg-ime/fcitx-qt5.git
	~~~

2.	修改最顶层的CMakeLists.txt，在 `set(CMAKE_MODULE_PATH ... ` 这句后面插入以下内容：

	~~~cmake
	set(Qt5Core_DIR /opt/Qt5.1.1/5.1.1/gcc_64/lib/cmake/Qt5Core/)
	set(Qt5Gui_DIR /opt/Qt5.1.1/5.1.1/gcc_64/lib/cmake/Qt5Gui/)
	set(Qt5Widgets_DIR /opt/Qt5.1.1/5.1.1/gcc_64/lib/cmake/Qt5Widgets/)
	set(Qt5DBus_DIR /opt/Qt5.1.1/5.1.1/gcc_64/lib/cmake/Qt5DBus/)
	~~~

	Qt5的路径修改为你电脑上的路径。

3.	正常编译

	~~~shell
	mkdir build
	cd build
	cmake ../
	make
	sudo make install
	~~~
