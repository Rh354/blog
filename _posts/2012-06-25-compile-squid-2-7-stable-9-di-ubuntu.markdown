---
layout: post
title: "Compile Squid 2.7 stable 9 di Ubuntu"
date: 2012-06-25 16:51
comments: true
categories: tutorial
tags: [linux, squid, tutorial]
fullview: false
---

Sebelum menginstall ada baiknya melakukan persiapan dulu
Persiapan :

1. PC yang memenuhi spek 32 bit tentunya

2. Backup squid.conf (kalau ada)

3. Minuman penambah smangat


Pastikan compilernya udah terinstal semua


{% highlight ruby %} sudo apt-get install gcc build-essential {% endhighlight %}

Jalankan perintah berikut di terminal untuk melihat informasi CPU lo

{% highlight ruby %}cat /proc/cpuinfo{% endhighlight %}

untuk pengguna AMD 64 bit bisa di lihat **[disini](http://en.gentoo-wiki.com/wiki/Safe_Cflags/AMD)** , sedangkan pengguna Intel **[disini](http://en.gentoo-wiki.com/wiki/Safe_Cflags/Intel)**
Catat informasi **CHOST** dan **CFLAGS** nya (sesuai dengan informasi cpu lo di ubuntu tadi), contoh gw menggunakan intel celeron M, maka gw memperoleh informasi **CHOST** dan **CFLAGS**nya.

{% highlight ruby %}CHOST="i686-pc-linux-gnu";
CFLAGS="-march=pentium-m -O2 -pipe -fomit-frame-pointer"{% endhighlight %}

Dowload squid 2.7stable9 dengan perintah :
{% highlight ruby %} wget http://www.squid-cache.org/Versions/v2/2.7/squid-2.7.STABLE9.tar.bz2{% endhighlight %}

lalu ekstrak dan masuk ke foldernya
{% highlight ruby %}tar xvjf squid-2.7.STABLE9.tar.bz2
cd squid-2.7.STABLE9{% endhighlight %}

**ok sekarang dimulai tahap compile nya**


masuk melalui terminal dimana folder squid tersebut berada dan jalankan command berikut : 

{% gist Rh354/739889ced39ad8c72a77 %}

>**WARNING !!!**

>1. Nilai diatas adalah contoh, nilai CHOST, CFLAGS **harus** sesuai dengan informasi CPU lo !
2. nilai ./configure hukumnya sunnah artinya bisa sama dengan yang diatas atau jika lo ingin berkreasi ketik ./configure -help dan lihat option2 lainnya.
3. jangan sampai salah mengetikkan ejaan, contoh enable-err-languages menggunakan "s" sedangkan yang di enable-default-err-language tidak menggunakan "s", tanya dah ma orang bule.


Install dah, ketik perintah berikut di terminal
{% highlight ruby %}sudo make
sudo make install{% endhighlight %}

segitu aja dl yah..konfigurasinya menyusul