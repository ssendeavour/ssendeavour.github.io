---
layout: post
title: "Add metadata(tag) for mp4 video file using avconv(aka. ffmpeg)为mp4文件添加标签"
tagline: "software, metadata, ffmpeg, avconv, video, tag, mp4"
description: "Add metadata(tag) for mp4 video file using avconv(aka. ffmpeg)为mp4文件添加标签"
tags: [software, metadata, ffmpeg, avconv, video, tag, mp4, python]
---
{% include JB/setup %}

为mp4视频文件添加标签等元信息(metadata)
---

我有一些mp4格式的歌曲，需要在播放歌曲的时候让OSDLyrics自动搜索显示对应的歌词。OSDLyrics和大部分视频/音频播放器是通过tag/metadata得到歌曲信息的（歌名、歌手、专辑等）。而绝大部分mp4视频文件是不带这些数据的。网上搜了一下，没找到好用的GUI软件，只好把寄希望于强大的FFmpeg上了。FFmpg（在Ubuntu里现在改名叫avconv了）可以在各种视频音频文件之间进行转换，同样也可以添加tag/metadata信息。

例如：

~~~shell
avconv -i input.mp4 \
-metadata title="圣哉三一歌" \
-metadata artist="新编赞美诗" \
-metadata album="新编赞美诗" \
-metadata track=1 \
-metadata comment="上海市基督教怀恩堂瓦器制作" \
-vcodec copy -acodec copy \
output.mp4
~~~

这样就为视频文件添加了tag信息，可喜的是，这样做的效率是非常高的，我写了个Python脚本处理的十几首mp4格式的歌曲，只用了几秒钟。之所以这样高效，原因就在我们选项的视频和音频编码器 `copy`，即只从输入流复制到输出流，不进行重新编码，这自然就快了。

不同文件格式支持的metadata也不尽相同，如果知道某种文件格式的`-metadata`选项后面用哪些tag呢，下面的网址有一份列表，但也有完全正确，如上面我使用的`artist`在列表上mp4 文件那里就没有，这是我自己猜出来的（列表中的`author`不正确，产生的文件vlc和mocp都无法读到artist的值。

[FFmpeg Metadata](http://wiki.multimedia.cx/index.php?title=FFmpeg_Metadata)

另外，从列表上可以看到， ffmpeg还支持mkv, mp3, avi, flv, mpg, wmv, wma 等常见格式。

##脚本自动化

我要转换的歌曲文件名有规律的，适合使用脚本处理。要处理的文件名如下：

~~~shell
新编赞美诗_001_圣哉三一歌_怀恩堂.mp4
新编赞美诗_307_欢乐颂扬歌_怀恩堂.mp4
新编赞美诗_362_收成归天家歌_怀恩堂.mp4
新编赞美诗_379_诗篇二十三篇_怀恩堂.mp4
新编赞美诗_382_诗篇一二一篇_怀恩堂.mp4
新编赞美诗_383_詩篇一三三篇.mp4
新编赞美诗_383_诗篇一三三篇_怀恩堂.mp4
新编赞美诗_384_詩篇一五零篇.mp4
新编赞美诗_384_诗篇一五〇篇_怀恩堂.mp4
新编赞美诗_短歌01_神爱世人_怀恩堂.mp4
新编赞美诗_短歌41_旧约目录歌_怀恩堂.MP4
新编赞美诗_短歌42_新约目录歌_怀恩堂.mp4
~~~

使用的Python脚本：

~~~python
# coding: utf8

import re
from os import listdir
from os.path import isfile, join
from subprocess import call

#regexp = re.compile(r'.*?_(.*?)_(.*?)(_|\.)(.*)')
regexp = re.compile(r'.*?(\d+)_(.*?)(_|\.)(.*)')

mypath = '/home/starfish/video/hymn-video'

filelist = [ f for f in listdir(mypath) if f.endswith(".mp4") and isfile(join(mypath, f)) ]

cmd = """avconv -i '%s' \
-metadata title='%s' \
-metadata artist="新编赞美诗" \
-metadata album="新编赞美诗" \
-metadata track=%s \
-metadata comment="上海市基督教怀恩堂瓦器制作" \
-vcodec copy -acodec copy \
'%s'
"""
for f in filelist:
	print f
	newfile = f.rsplit('.')[0] + "_metadata.mp4"
	print newfile
	result = regexp.findall(f);
	if result:
		for idx, s in enumerate(result[0]):
			print idx, ":", s
		fullcmd =  cmd % ( f, result[0][1], result[0][0], newfile)
		call(fullcmd, shell=True)
~~~

如果你的文件名没有这么有规律，恐怕就要手工修改了。。。不过还是可以写个脚本提示输入各种参数，或者先编辑好所有命令，再一次运行。这总比每次在命令行上翻出历史-->左右移动光标编辑-->运行快的多了。
