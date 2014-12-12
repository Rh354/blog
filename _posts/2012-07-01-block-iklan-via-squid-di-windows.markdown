---
layout: post
title: "Block Iklan Via Squid di Windows"
date: 2012-07-01 15:00
comments: true
categories: tutorial
tags: [tutorial, squid, windows]
fullview: false
---


Berikut kita akan memblock iklan melalui squid di windows
buka link berikut **[ini](http://tinypaste.com/4ef73)**
save dengan nama **ads.block** di C:\squid\etc\
kalau udah silahkan tambahkan ini di squid.conf

{% highlight ruby %}
# ADBLOCKING
acl iklan url_regex -i "c:/squid/etc/ads.block"
deny_info http://lusca.indolini.org/fill.png iklan

http_access deny iklan
{% endhighlight %}

silahkan lakukan

{% highlight ruby %}
squid -k parse
{% endhighlight %}

untuk memastikan tidak ada error sebelum di eksekusi & lakukan

{% highlight ruby %}
squid -k reconfigure
{% endhighlight %}

ato restart squidnya sekalian apabila tidak ada error

testing dengan membuka **kompas.com** atau **detik.com** di browser selain firefox dan bandingkan sebelum dan sesudah memakai proxy squid di windows

Thanks to**[Th30nly @ KASKUS]("http://www.kaskus.us/member.php?u=113821)** untuk lebih lanjut silahkan ke official thread di **[Comstuff](http://www.comstuff.net/showthread.php?t=239)**
