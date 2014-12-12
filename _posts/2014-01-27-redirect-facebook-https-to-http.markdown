---
layout: post
title: "redirect facebook https to http"
date: 2014-01-27 10:48:09 +0700
comments: true
fullview: false 
categories: tutorial
tags: [windows, tutorial, linux]
---
Beberapa HTTPS masih bisa dipaksa untuk melayani request HTTP dengan melakukan redirect dari HTTPS ke HTTP dan dalam kasus ini adalah facebook, dengan harapan kontennya dapat di cache oleh squid proxy. 
tutorial ini berlaku untuk Firefox, chrome, Opera, Namun yang browser yang dicoba dalam artikel ini adalah firefox dan coolnovo (browser turunan dari chrome dengan fitur proxy sendiri berbeda dari chrome).

1. Install Addon firefox melalui link [https everywhere](https://www.eff.org/https-everywhere) dan restart browser anda.
2. Install Addon chrome melalui link [https everywhere](https://www.eff.org/https-everywhere) dan restart browser anda.
3. Download file redirect xml dari **[reges007](http://www.kaskus.co.id/profile/2669931)** isi dari redirector tersebut seperti ini 

{% gist Rh354/1cfbce9afaf7387c3734 %}


###Setting di mozilla firefox (diutamakan versi stable terbaru)###
- buka Firefox dan ketik `about:support` di address bar firefox anda
- pada `Profile Folder` pilih `show folder` kemudian minimize foldernya
- copy `redirector.xml` yang telah di download tadi ke dalam **folder https everywhere** yang akan muncul setelah install https everywhere
- buka firefox dan klik **tools-> add-ons** pilih **options** pada add-ons **https-everywhere**	
- pilih `disable all` dan pada search cari **reges7** lalu aktifkan `reges7` dengan cara **klik 2x tanda X** ke tanda Yes(warna hijau)
- restart firefox dan buka facebook. untuk mengeceknya liat file access.log squid.

##Setting di Google Chrome###
- ceklist show hidden pada tools di windows untuk menampilkan
- cari file `default.rulesets` 

di xp dan menggunakan browser google chrome 

{% highlight ruby %}
C:\Documents and Settings\User(nama administrator/pc)\Local Settings\Application Data\Google\Chrome\User Data\Default\Extensions\gcbommkclmclpchllfjekcdonpmejbdp\2014.1.3_0\rules 
{% endhighlight %}

Di Windows 7 dan menggunakan browser google chrome

{% highlight ruby %}
C:\User\User(nama administrator/pc)\AppData\Local\Google\Chrome\User Data\Default\Extensions\gcbommkclmclpchllfjekcdonpmejbdp\2014.1.3_0\rules 
{% endhighlight %}

Di windows 7 dengan menggunakan browser coolnovo 

{% highlight ruby %}
C:\Users\User(nama administrator/pc)\AppData\Local\MapleStudio\ChromePlus\User Data\Default\Extensions\gcbommkclmclpchllfjekcdonpmejbdp\2014.1.3_0\rules
{% endhighlight %}

- backup terlebih dahulu rulesetnya lalu edit default rulesetsnya, hapus isi aslinya kemudian masukan rule `reges7`
- restart coolnovo/chrome dan buka facebook. untuk mengeceknya liat file access.log squid.

**Note : 2014.1.3_0 adalah tanggal pada saat extensi ini diinstall**




