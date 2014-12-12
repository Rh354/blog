---
layout: post
title: "Mengaktifkan Traceroute Nagios Via NRPE Nagios"
date: 2014-02-02 21:05:03 +0700
comments: true
categories: tutorial
tags: [linux, tutorial, nagios]
fullview: false  
---
Lanjutan dari artikel sebelumnya yaitu [menambah plugin traceroute di nagios]({% post_url 2014-01-29-menambah-plugin-traceroute-di-nagios %}), kali ini via `plugin nrpe` nagios, casenya adalah ada 2 link internet, link pertama dapat dihandle oleh nagios dan link kedua ada di router kedua dan si nagios tidak ada akses ke link kedua. namun di salah satu server yang dimonitoring ada akses untuk link kedua tersebut. setelah mencoba sana sini akhirnya diputuskan untuk mencoba traceroute via nrpe nagios di server lain.

**asumsinya adalah plugin nrpe sudah berfungsi di server yang akan dijadikan monitoring traceroute.**

save as lah code di bawah ini dengan nama check_traceroute di folder nagios plugin anda. di artikel ini kebetulan ada di `/usr/lib/nagios/plugins/`

{% gist Rh354/859ab39984d7b381ca83 %}

{% highlight ruby %}
chmod +x /usr/lib/nagios/plugins/check_traceroute
{% endhighlight %}

Jika mengalami error tentang `/usr/bin/gethostip` (error yang terjadi ketika artikel ini dibuat dengan menggunakan opensuse) maka silakan menginstall paket `syslinux` via **yast** ataupun **zypper** 

dan restart nrpe anda dengan perintah

{% highlight ruby %}
/etc/init.d/nrpe restart
{% endhighlight %}

jika anda melakukan 

{% highlight ruby %}
/usr/lib/nagios/plugins/check_traceroute 8.8.8.8
{% endhighlight %}

dan hasilnya `traceroute OK: 26.119 ms 24.579 ms 23.041 ms`

artinya command tersebut jalan namun belum terintegrasi oleh plugin nrpe

untuk mengetes jalan atau tidaknya command yang anda masukkan ke nrpe anda dapat memakai command berikut

{% highlight ruby %}
/usr/lib/nagios/plugins/check_nrpe -H hostname -c check_mycommand
{% endhighlight %}

di dalam artikel ini menjadi

{% highlight ruby %}
/usr/lib/nagios/plugins/check_nrpe -H 127.0.0.1 -c check_traceroute
{% endhighlight %}

jika error maka ada error seperti berikut `NRPE: Command 'check_traceroute' not defined`

yang harus dilakukan selanjutnya ialah melakukan integrasi plugin nrpe agar dapat mengenali command `check_traceroute`

untuk itu kita harus mengedit file konfigurasi nrpe dengan editor kesayangan anda disini saya memakai `nano`

{% highlight ruby %}
 nano /etc/nagios/nrpe.cfg
{% endhighlight %}

lalu tambahkan baris berikut

{% highlight ruby %}
command[check_traceroute]=/usr/lib/nagios/plugins/check_traceroute 8.8.8.8 #8.8.8.8 adalah ip internet yang akan dituju
{% endhighlight %}

restart kembali nrpe anda dengan perintah

{% highlight ruby %}
/etc/init.d/nrpe restart
{% endhighlight %}

lalu cek kembali command tersebut dengan perintah berikut

{% highlight ruby %}
/usr/lib/nagios/plugins/check_nrpe -H 127.0.0.1 -c check_traceroute
{% endhighlight %}

jika hasilnya `traceroute OK: 18.182 ms 19.413 ms 20.865 ms` maka selamat anda telah berhasil
