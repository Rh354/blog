---
layout: post
title: "Cache Dir di Squid"
date: 2012-06-30 16:38
comments: true
categories: tutorial
tags: [tutorial, linux, proxy, windows]
fullview: false
---


Option pada cache_dir menentukan sistem penyimpanan seperti apa yang akan digunakan (aufs,ufs), nama direktori tempat penyimpanan cache *(/var/cache/squid)*, ukuran disk dalam megabytes yang digunakan oleh direktori tempat penyimpanan cache (1600 Mbytes), jumlah subdirektori pertama yang akan dibuat di bawah /var/cache/squid ,dan jumlah subdirektori kedua yang akan diciptakan di bawah subdirektori pertama tadi (256).

Nilai2 pada option cache_dir tadi harus disesuaikan dengan sistem yang anda miliki, biasanya yang harus disesuaikan hanyalah tempat penyimpanan cache, ukuran disk, dan jumlah subdirektori yang akan dibuat. Mengenai angka2 tersebut, dapat kita peroleh dari rumus yang telah disediakan untuk optimasi sbb:


1. Gunakan 80% atau kurang dari setiap kapasitas cache direktori yang telah kita siapkan. Jika kita mengeset ukuran cache_dir kita melebihi nilai ini,maka kita akan dapat melihat penurunan performansi squid.
2. Untuk menentukan jumlah subdirektori pertama yang akan dibuat, dapat menggunakan rumus ini:

{% highlight ruby %}
x= Ukuran cache dir dalam KB (misal 6GB=~6,000,000KB)
y= Average object size (gunakan saja 13KB)
z= Jumlah subdirektori pertama = (((x / y) / 256) / 256) * 2 = # direktori
{% endhighlight %}

Sebagai contoh, misal saya menggunakan *6 GB dari untuk /cache (setelah disisihkan 80% nya)*, maka:

{%highlight ruby %}
6,000,000 / 13 = 461538.5 / 256 = 1802.9 / 256 = 7 * 2 = 14
{% endhighlight %}

maka baris cache_dir akan menjadi seperti ini: `cache_dir ufs /var/cache/squid 6000 14 256`

