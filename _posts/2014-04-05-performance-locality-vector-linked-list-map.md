---
layout: post
title: "Modern C++: What You Need to Know"
date: Sat Apr  5 15:53:25 CST 2014
tags: [Performance, C++]
categories: C++
code: 1
mathjax: 0
---
{% include JB/setup %}

---

来自微博Build 2014演讲视频 **Modern C++: What You Need to Know** by Herb Sutter

地址：<http://channel9.msdn.com/Events/Build/2014/2-661?format=html5>

slide: <http://video.ch9.ms/sessions/build/2014/2-661.pptx>

视频：<http://video.ch9.ms/sessions/build/2014/2-661_LG.mp4>

---

### 局部性 与 vector

![](/images/posts/locality_vector.png)

右边的代码比左边的快**50**倍。

---

### vector, map, list 在中间做插入、删除的性能

![](/images/posts/vector_list_mid_insert_delete.png)


linked list 慢的最主要原因是它只有顺序的查找插入/删除点。而vector可以用二分。

**所以：**

- 默认使用 `vector<T>`
- 需要查找的(dictionary lookup)使用 `map<key,val>`(tree-based)或者`unordered_map<key,val>`(hash-based)
- 复杂度理论只是非常概略的指导。

-------

这让我想到了另一个例子，它是stackoverflow C++标签下评分最高的问题，得了7497分（写这篇博客时）。这个问题是关于`Branch Predication`的，在《深入理解计算机系统》(*Computer system: a programmer's perspective*)里有讲到。上面的局部性当然也讲了。

[Why is processing a sorted array faster than an unsorted array?](http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array)
