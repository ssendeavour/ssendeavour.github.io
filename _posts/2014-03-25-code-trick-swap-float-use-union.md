---
layout: post
title: "Code Trick: swap float using union"
date: Tue Mar 25 16:11:36 CST 2014
tags: [trick, c]
categories: C
code: 1
mathjax: 0
---
{% include JB/setup %}

精彩代码：交换float的endian
---

使用C语言Union交换float的各字节（转换endian）。来自<http://www.gamedev.net/page/resources/_/technical/game-programming/writing-endian-independent-code-in-c-r2091>

---

~~~c
float FloatSwap( float f )
{
  union
  {
    float f;
    unsigned char b[4];
  } dat1, dat2;

  dat1.f = f;
  dat2.b[0] = dat1.b[3];
  dat2.b[1] = dat1.b[2];
  dat2.b[2] = dat1.b[1];
  dat2.b[3] = dat1.b[0];
  return dat2.f;
}
~~~
