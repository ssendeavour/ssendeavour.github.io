---
layout: post
title: "Install jekyll github page in Ubuntu 12.04"
date: Sat Mar  1 14:23:45 CST 2014
tags: [Ubuntu, Github, Software]
categories: Software & Tool
code: 1
mathjax: 0
---
{% include JB/setup %}

在Ubuntu 12.04下安装jekyll github page 写博客
---

注：要在github page写博客，首先要建好以你的github用户名+"github.io"为名的特殊repository。本文假设你已经建好了这个repository。

---
jekyll 是用ruby写的，因此需要先安装ruby和相关的软件。


### 安装ruby

ruby的版本需要>= `1.9.3`或者`2.0.0`，Ubuntu 12.04 源里的ruby版本比这个低，我使用`rvm`来安装最新的ruby。

- 下载安装 `rvm`

~~~
\curl -sSL https://get.rvm.io | bash
~~~

- 查看有哪些版本的ruby可以安装

~~~
rvm list knwon
~~~

- 根据上面列出的ruby版本，选一个满足要求的版本安装，我安装的是2.1.1

~~~
rvm install 2.1.1 -C --with-iconv-dir=$HOME/.rvm,--with-readline
~~~

这个步骤中如果 apt-get update 命令执行不成功，安装将会失败。

至此，就安装好了ruby。

### 安装 Bundler

它是一个包管理器，用于管理Jekyll

~~~
gem install bundler
~~~

### 正题：安装 Jekyll

在你的博客版本库的根目录下（本博客是ssendeavour.github.io这个目录）创建一个名为`Gemfile`的文件，文件内容为：

~~~
source 'https://rubygems.org'
gem 'github-pages'
~~~

然后在同一目录运行命令

~~~
bundle install
~~~

经过一连串的Installing屏显，jekyll 就安装好了，接下来就可以运行jekyll了。

### 运行 jekyll 服务器

~~~
bundle exec jekyll serve -w
~~~

现在打开浏览器，输入地址`127.0.0.1:4000`，就能看到你的博客已经运行起来啦。

### 更新 jekyll
Github page 服务器上的jekyll是随时更新的，所以我们自己电脑上的jekyll也要与github服务器上的版本一致，以免出现问题。

更新jekyll的命令

~~~
bundle update
~~~

### mathjax/LaTeX

需要先加载 mathjax.js，官方教程<http://docs.mathjax.org/en/latest/start.html>

- displayed mathematicas: `$$ .. $$` or `\\[ .. \\]`
- inline mathematics: `\\( .. \\)`, **note**: `$ .. $` is not enabled by default.

-------

这样，jekyll就安装好了，后续的配置还请Google，别人写了很多了。

其实这份教程是写给我自己的，重装系统后我的配置文件都可以用git clone 从github 上同步下来，所以现在我不需要关系怎样配置了，只需要按上面的步骤把jekyll安装到电脑里就可以开始写博客了。

---
参考资料：
- <https://help.github.com/articles/using-jekyll-with-pages>
- <http://jekyllrb.com/docs/templates/>
