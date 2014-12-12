---
layout: post
title: "Dig di Windows"
date: 2012-06-30 17:23
comments: true
categories: tutorial
tags: [tutorial, windows, dns]
fullview: false
---


Dig adalah sebuah tools untuk melihat query dari sebuah DNS yang sedang dipakai. contoh ketika memakai **[unbound]({% post_url 2012-06-30-install-unbound-di-windows %})** atau **[BIND]({% post_url 2014-10-03-install-bind-di-windows %})** di windows

Ketika melakukan pegecekan DNS/IP suatu domain, di keluarga *NIX biasanya bisa langsung di pakai misal pakai aplikasi dig. 

Di keluarga MS Windows secara default tidak tersedia. tapi setelah browsing sana sini ternyata dapat juga berbagai macam tools untuk dig tersebut cara penggunaannya pun gampang. ada yang lewat CMD atau command line maupun memakai aplikasi berbasis GUI

untuk dig berbasis CMD silahkan ikuti langkah ini :

- Download **[dig](https://www.mediafire.com/?kcy40yd3r39ao47)**
- ekstrak semua file tersebut kedalam satu folder semisal dig
- klo udah kopi paste seluruh file di folder dig(inget file yak bukan folder) di **C:\Windows\System32**


beres deh

sekarang tinggal lakukan dig di **CMD** misal **dig google.com**

bagi yang seneng memakai interface bisa mencoba [Ezdig](http://www.eztk.com/products/ezdig.php)

Cukup mudah di gunakan tinggal masukan dns server yang akan dipakai atau gunakan yang default (mengikuti seting dns di pc kita). Kemudian isikan nama domain yang mau di resolv pada kolom hostname kemudian klik **GO**
