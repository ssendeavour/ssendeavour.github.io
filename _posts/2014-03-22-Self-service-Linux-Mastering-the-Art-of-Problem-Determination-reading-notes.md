---
layout: post
title: "Self service Linux: Mastering the Art of Problem Determination Reading Notes"
date: Sat Mar 22 14:48:02 CST 2014
tags: [Linux, debug]
categories: Linux
code: 1
mathjax: 0
---
{% include JB/setup %}

Self service Linux 读书笔记
---

作者：Mark Wilding and Dan Behman
出版日期：2005.9

简介摘要：

> In a nutshell, this book is about effectively and efficiently diagnosing problems that occur in the Linux environment. It covers good investigation practices, how to use the information and resources on the Internet, and then dives right into detail describing how to use the most important problem determination tools that Linux has to offer.
> Ultimately, as Linux increases in popularity, there are many seasoned experts who are facing the challenge of translating their knowledge and experience to the Linux platform. Many are already experts with one or more operating systems except that they lack specific knowledge about the various command line incantations or ways to interpret their knowledge for Linux. This book will help such experts to quickly adapt their existing skill set and apply it effectively on Linux.

---

## 第1章

### 需要安装的软件：
- `strace` : 跟踪系统调用
- `ltrace` : 跟踪函数调用（与`strace`类似，但更详细）
- `lsof` : lists open files
- `top` : list running process
- `traceroute`/`tcptraceroute` : 跟踪网络路由
- `ping` : 测试网络连通性
- `hexdump`或类似软件 : 以16进制显示文件内容
- `tcpdump`和/或`ethereal` : display packets of network traffic
- `GDB` : 调试器
- `readelf` : 显示`ELF`格式可执行文件信息

这些工具（还有更多）都列在附录A中。




