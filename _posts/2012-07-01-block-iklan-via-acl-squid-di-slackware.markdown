---
layout: post
title: "Block Iklan Via ACL Squid Di Slackware"
date: 2012-07-01 14:42
comments: true
categories: tutorial
tags: [tutorial, linux, squid]
fullview: false
---


Berikut ini kita akan memblok iklan melalui squid di slackware (dapat diterapkan di lingkungan server *nix)
sebelumnya masuk dl ke root melalui **su** di terminal


{% highlight ruby %}
# touch /etc/squid/ads.block
{% endhighlight %}

ato bisa memakai sudo

{% highlight ruby %}
$ sudo touch /etc/squid/ads.block
{% endhighlight %}

edit file tersebut dengan editor yang lo suka

{% highlight ruby %}
# nano /etc/squid/ads.block
{% endhighlight %}

lalu isi file tersebut dengan regex yang ada di link berikut **[ini]("http://www.comstuff.net/showthread.php?t=239) (Updated)**

tambahkan rules ini di squid.conf

{% highlight ruby %}
# ADSBLOCKING
acl iklan url_regex -i "/etc/squid/ads.block"
deny_info http://lusca.indolini.org/fill.png iklan

http_access deny iklan
{% endhighlight %}

kalau udah silahkan lakukan 

{% highlight ruby %}
squid -k parse 
{% endhighlight %}

lalu 

{% highlight ruby %}
squid -k reconfigure
{% endhighlight %}
 
untuk testing silahkan buka **kompas.com** atau **detik.com** di browser selain firefox lalu bandingkan sebelum dan sesudah memakai squid
