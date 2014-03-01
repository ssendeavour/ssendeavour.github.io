---
layout: post
title: "Play DVD in Ubuntu -- instructions from Ubuntu community wiki"
date: Sat Mar  1 14:04:46 CST 2014
tags: [Ubuntu, DVD, media]
categories: Misc
code: 1
mathjax: 0
---
{% include JB/setup %}

在Ubuntu 12.04中播放DVD（来自Ubuntu社区wiki）
---

虽然很少播放DVD，但有备无患，还是预备着。网上也有教程，但都没有提到wiki里提到的一步。所以特地在这里记下来。

---

大多数商业DVD都使用CSS加密了，安装`libdvdcss2`后可以播放这些加密的DVD。

~~~
sudo apt-get install libdvdread4
~~~

安装后，运行命令

~~~
sudo /usr/share/doc/libdvdread4/install-css.sh
~~~

现在VLC就可以播放加密的DVD了。

----
参考：
- <https://help.ubuntu.com/community/RestrictedFormats/PlayingDVDs>


