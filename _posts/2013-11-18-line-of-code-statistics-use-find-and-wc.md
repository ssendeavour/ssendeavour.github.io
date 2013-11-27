---
layout: post
title:  "use find and wc to do line of code statistics"
date: Mon Nov 18 11:33:22 CST 2013
tags: [Shell, oneliner, find, LOC, Linux]
categories: Shell
---
{% include JB/setup %}
使用find和wc命令统计代码行数
---

---

It's simple, suppose we want to count *.cpp and *.h, run this oneliner in the root directoy of your project:

~~~shell
find . \( -name '*.cpp' -o -name '*.h' \) -print0 | wc -l --files0-from=- | sort  -n
~~~

sample output:

~~~shell
➜  SchoolPersonnelMgmt git:(master) find . \( -name '*.cpp' -o -name '*.h' \) -print0 | wc -l --files0-from=- | sort -n
7 ./StudentFilterDialog.cpp
18 ./CommonSortFilterProxyModel.h
22 ./main.cpp
25 ./CommonSortFilterProxyModel.cpp
25 ./StudentTableDelegate.h
26 ./StudentFilterDialog.h
32 ./teachingassistant.h
35 ./const.h
43 ./teachingassistant.cpp
46 ./PostgraduateTableModel.h
46 ./TeacherTableModel.h
46 ./TeachingAssistantTableModel.h
48 ./teacher.cpp
49 ./StudentTableModel.h
50 ./postgraduate.cpp
56 ./student.h
66 ./teacher.h
68 ./mainwindow.h
71 ./postgraduate.h
78 ./person.cpp
88 ./student.cpp
142 ./StudentTableDelegate.cpp
148 ./person.h
198 ./PostgraduateTableModel.cpp
198 ./TeacherTableModel.cpp
198 ./TeachingAssistantTableModel.cpp
216 ./StudentTableModel.cpp
404 ./mainwindow.cpp
2449 total
~~~
