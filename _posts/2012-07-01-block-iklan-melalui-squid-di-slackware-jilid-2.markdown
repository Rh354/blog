---
layout: post
title: "Block Iklan Melalui Squid di Slackware Jilid 2"
date: 2012-07-01 15:44
comments: true
categories: tutorial
tags: [tutorial, linux, squid]
fullview: false
---


hehehe mengapa jilid 2 karena sebelumnya udah gw post cara ngeblok iklan melalui squid. nah sekarang jg ada lagi cara ngeblok iklan melalui cara lain,cara ini jg bisa digabung dengan cara satunya koq

buat sebuah script seperti ini :

{% gist Rh354/c73d704ecb169af0b168 %}

save script itu dengan nama sembarang,misal di gw **update-iklan-squid.sh** lalu dibuat executable

{% highlight ruby %}
$ sudo chmod +x update-iklan-squid.sh
{% endhighlight %}

asumsi perintah diatas adalah file **update-iklan-squid.sh** berada di home lo

untuk distro selaen slackware yang menggunakan **init.d** silahkan menyesuaikan dibagian **reloadcmd**

klo udah buka squidnya

{% highlight ruby %}
sudo vi /etc/squid/squid.conf
{% endhighlight %}

lalu masukkan rules ini (klo gw taro di acl)

{% highlight ruby %}
acl ads dstdom_regex -i "/etc/squid/squid.adservers"
deny_info http://lusca.indolini.org/fill.png ads

http_access deny ads
{% endhighlight %}

klo udah langsung eksekusi scriptnya (asumsi scriptnya ada di folder home)
{% highlight ruby %}
sudo ./update-iklan-squid.sh
{% endhighlight %}

klo udah tinggal buat cronnya aja sesuai selera lo
