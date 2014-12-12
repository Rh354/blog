---
layout: post
title: "install python di windows 7"
date: 2014-01-22 09:21:30 +0700
comments: true 
fullview: false
categories: tutorial
tags: [windows,tutorial]
---

Dalam artikel ini saya menggunakan Windows 7 64bit namun python yang saya install adalah installer 32bit dengan versi 2.7.x. 
Beberapa alasannya adalah :

- versi phyton 64bit masih ada masalah kompatibilitas dengan beberapa plugin dari pythonnya
- versi 2.7.x saya gunakan karena hanya sebatas untuk dipakai dalam instalasi octopress yang akan di jelaskan di artikel berikutnya
- versi 3.x sampai saat artikel ini ditulis juga masih ada yang blm support beberapa plugin yang mungkin nanti akan dibutuhkan

well, mungkin langsung saja.

- kita download dahulu pythonnya dari **[sini](http://python.org/download/)**, saya sendiri memilih Python 2.7.6 Windows Installer pada saat artikel ini dibuat.
- install secara default, defaultnya akan ada di path berikut {% highlight ruby %}C:\Python27{% endhighlight %}
- selanjutnya kita masukkan path python tersebut agar system windows dapat mengenali executable python dengan cara klik kanan **My computer - properties - pilih advanced system settings** di sebelah kiri. lalu klik **environment variables** pada bagian **User Variables** edit bagian **PATH** dan masukkan path berikut
{% highlight ruby %}C:\Python27;C:\Python27\Lib\site-packages\;C:\Python27\Scripts\;{% endhighlight %}
- untuk test silakan buka command prompt dengan cara **start menu - all program - accessories** atau **start menu - run** dan ketik **cmd**, jika telah terbuka ketik **python** maka akan terbuka seperti ini
{% highlight ruby %}Python 2.7.6 (default, Nov 10 2013, 19:24:18) [MSC v.1500 32 bit (Intel)] on win
32
Type "help", "copyright", "credits" or "license" for more information.
>>>{% endhighlight %} jika muncul seperti itu artinya instalasi sudah sukses, untuk keluar dari python tersebut silakan tekan **"CTRL Z"** (tanpa tanda petik) dan tekan enter.

selanjutnya kita menginstall beberapa plugin atau paket yang di perlukan:
- untuk windows kita tinggal download dan save script yang telah dibuat di link **[ini](https://pypi.python.org/pypi/setuptools#windows)**
- jalankan script **ez_setup.py** yang telah di download di link sebelumnya melalui command prompt. contoh saya menaruh script tersebut di drive **D** maka saya tinggal membuka command prompt dan lakukan drag and drop script **ez_setup.py** ke command prompt dan tekan enter atau anda ketik manual dengan cara berikut

{% highlight ruby %}C:\> D: #untuk berpindah dari drive C ke drive D via command prompt{% endhighlight %} lalu ketik ez_setup.py dan tekan enter{% highlight ruby %}D:\> ez_setup.py{% endhighlight %}

tunggu hingga selesai dan kita siap melangkah untuk instalasi octopress di windows di artikel selanjutnya di artikel [octopress di windows](/octopress-di-windows/).


Referensi lebih lanjut silakan baca link berikut:  

[http://www.anthonydebarros.com/2011/10/15/setting-up-python-in-windows-7/](http://www.anthonydebarros.com/2011/10/15/setting-up-python-in-windows-7/)




