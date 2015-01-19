---
layout: post
title: "Install Unbound Di Windows"
date: 2012-06-30 17:54
comments: true
categories: tutorial
tags: [tutorial, dns, windows]
fullview: false
---


sebelumnya gw udah memposting mengenai cara compile dan installasi unbound di **[slackware]({{site.baseurl}}{% post_url 2012-06-29-compile-unbound-di-slackware %})** dan **[ubuntu]({{site.baseurl}}{% post_url 2012-06-30-install-unbound-di-ubuntu %})**. sekarang saatnya instalasi di windows

berikut caranya :

1. Download dulu unboundnya di websitenya lsg **[dimari](http://unbound.net/download.html)**
2. lalu buat file **service.conf** atau edit jika telah ada di **C:\Program Files\unbound** jika memakai 32bit atau**C:\Program Files (x86)\unbound**

ini settingan gw untuk 32bit

{% gist Rh354/48c0a84473c906936764 %}

dan ini settingan gw untuk 64bit

{% gist /Rh354/15f69ab1a5dc0e56b16d %}

2. jalankan deh unboundnya lewat service management windows pasti lsg error

untuk mengatasinya perlu 4 file yang di generate khusus oleh sistem *nix

dibawah ini dah gw sertakan 4 file tersebut beserta isinya

silahkan dibuat sendiri melalui notepad++

{% gist Rh354/a821feaf54b25a31be58 %}

save dengan nama unbound_control.key

{% gist Rh354/917d6c3dc8d934d3faef %}

save dengan nama unbound_server.pem

{% gist Rh354/16d9cdd87565071c523d %}

save dengan nama unbound_control.key

{% gist Rh354/6de8a5228c516f8a305f %}

save dengan nama unbound_control.pem


silahkan jadikan satu di folder unbound

klo udah lakukan pengecekan di unboundnya melalui CMD di 

{% highlight ruby %}
C:\Program Files\Unbound>unbound-checkconf.exe
{% endhighlight %}

klo misalnya hasilnya no error berarti dah beres klo misal ada error akan diberitahu cluenya

klo udah silahkan restart unboundnya di service management dengan klik **start menu - run** lalu ketik **services.msc** cari dah unboundnya disana & tinggal klik restart

langkah selanjutnya :

1. Klik kanan di LAN atau Wireless Network Connection, lalu pilih properties. Tergantung kalian pake LAN atau WIFI
2. Lalu klik di internet protocol (TCP/IP) klik properties
3. Lalu pilih Use the following DNS server addresses
4. Masukkan preferred DNS server: 127.0.0.1
5. Lalu klik ok
6. lalu klik kanan di gambar jaringan di paling bawah kanan layar komputer kita, lalu pilih repair

**NOTE :**

1. DNS diatas blm ditambahkan dengan **dns nawala** 180.131.144.144; 180.131.145.145
2. jika ingin mengganti DNS maupun menambah silahkan merubah atau menambah di bagian forwarder zone
3. untuk melalukan pengecekan silahkan melakukan dig. klo belum punya silahkan **[kemari]({{site.baseurl}}{% post_url 2012-06-30-dig-di-windows %})**
4. bagi pemakai squid silahkan ditambahkan confignya dengan :

{% highlight ruby %}
dns_nameservers 127.0.0.1
{% endhighlight %}

5. Tested under Windows XP, Windows 7

