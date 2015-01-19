---
layout: post
title: "Squid Untuk Windows"
date: 2014-02-12 09:29:13 +0700
comments: true
categories: tutorial
tags: [windows,squid,tutorial]
fullview: false
---
Artikel kali ini saya akan share sebuah program caching yang bernama **squid**. squid yang saya share ini khusus saya compile dari source nya dengan ditambah bumbu - bumbu agar menyamai kemampuan saudaranya yaitu lusca **(konfigurasi refresh pattern lusca yang aggressive dengan penambahan bumbu juga bisa di pakai di squid ini)** dan bisa berjalan di lingkungan windows, ketika pembuatan artikel ini yang telah di test adalah **windows xp** dan **windows 7 64bit dengan UAC Off**. namun jika ingin memakai alternatif yang sudah jadi dan tinggal pakai silakan lihat langkah selanjutnya.

**Alternatif lain** jika tidak ingin melakukan langkah di bawah silakan download installer yang telah jadi di link berikut [http://www.comstuff.net/showthread.php?t=12](http://www.comstuff.net/showthread.php?t=12)

**kelebihannya adalah :** 

- new engine support XP - Win8.1 (x32 and x64).
- latest storeurl.exe (07092014).
- adsblock (ads image, html, popup).
- adsblock updater menu.
- unbound install included.
- force youtube 240p.
- bypass dns blocking from ISP

untuk link alternatif diatas jika ingin menggunakan alternatif konfigurasi silakan menggunakan konfigurasi berikut

{% gist Rh354/67ea4f4233cee2e79f51 %}

berbeda dengan squid yang saya compile dibawah (port 3128), installer ini defaultnya menggunakan port 8000 namun bisa di edit jika ingin memakai port 3128 (disamakan dengan port squid)

namun jika tetap ingin melakukan secara manual berikut langkahnya :

1. Download paket yang udah di sediakan melalui link berikut [http://www.mediafire.com/download/s06w7aqaavfqkao/squid-30082014.7z](http://www.mediafire.com/download/s06w7aqaavfqkao/squid-30082014.7z) dimohon untuk yang mendownload agar tidak melakukan mirroring terhadap file ini. jika nantinya tidak ada silakan leave comment agar dilakukan reupload.
2. lalu downlod storeurl.exe (dengan file ini kita tidak perlu menginstall perl software) yang udah di sediakan melalui link berikut [http://www.mediafire.com/download/okxtvp0i8byx6gk/storeurl-30082014.7z](http://www.mediafire.com/download/okxtvp0i8byx6gk/storeurl-30082014.7z) dimohon untuk yang mendownload agar tidak melakukan mirroring terhadap file ini. jika nantinya tidak ada silakan leave comment agar dilakukan reupload.
3. Extrak file squid-30082014.7z di drive C sehingga menjadi `C:\squid` yang didalamnya ada folder `bin,etc,libexec,sbin,share,var`
4. Extrak file storeurl-30082014.7z di folder C:\squid\etc sehingga menjadi `C:\squid\etc\storeurl.exe`
5. Save as squid.conf dibawah ini (anda di beri kebebasan untuk mengganti, menambah maupun mengurangi ini dari squid.conf) ke folder `C:\squid\etc` namun sebelum mendownload anda harus pahami dulu beberapa bagian yang mungkin akan anda edit menyesuaikan kebutuhan anda
6. Buatlah folder `youtube` (jika belum ada) di `C:/squid/var/logs`

drive yang digunakan untuk `cache log` squid dalam artikel ini adalah `drive D` dan jika ingin diganti silakan mengganti bagian **cache_dir,coredump_dir,access_log,cache_log** di `squid.conf`.

di dalam artikel ini besarnya squid cache adalah **6 Gb (6000)** silakan disesuaikan dengan kondisi masing - masing dan mungkin link [cache dir di squid]({{site.baseurl}}{% post_url 2012-06-30-cache-dir-di-squid %}) dapat membantu

{% gist Rh354/7dd06d8ff27fc3f3bde9 %}

jika sudah edit environment variables anda untuk memasukkan path nya squid **(cara ini sangat penting untuk dilakukan karena akan mempengaruhi langkah selanjutnya)**. caranya klik kanan `my computer atau computer` di start menu lalu klik `properties` klik `advanced system settings` dan klik `environment variables` dan pada `system variables` cari variable `path` lalu isi seperti berikut

{% highlight ruby %}
;C:\squid\bin;C:\squid\sbin
{% endhighlight %}

jika sudah di **OK** saja.

langkah selanjutnya buka `Command Prompt` dengan cara `start menu` klik `run` dan ketik `CMD`

untuk mendaftarkannya squid agar menjadi service maka ketik command berikut di cmd tersebut

{% highlight ruby %}
squid -i
{% endhighlight %}

dan masukkan command berikutnya agar squid tidak error ketika tidak ada koneksi

{% highlight ruby %}
squid -O -DY
{% endhighlight %}

lalu untuk membuat directory squid maka ketik command berikut

{% highlight ruby %}
squid -z
{% endhighlight %}

jika terdapat error silakan cek dengan perintah berikut dan nanti akan di beri cluenya yang error dimana

{% highlight ruby %}
squid -k parse
{% endhighlight %}

jika tidak ada error langkah selanjutnya adalah melakukan start service squid

{% highlight ruby %}
net start squid
{% endhighlight %}

sampai disini silakan cek file `cache.log`. seharusnya disana sudah ada keterangan start

beberapa perintah lainnya yang berguna dan yang mesti diingat ialah

Melakukan debugging apabila squid error tapi ga' tau salah dimana, maka jalankan perintah ini

{% highlight ruby %}
squid -d 1 D
{% endhighlight %}

perintah itu akan melakukan trace error nya ada di mana, jika sudah kopi seluruh hasilnya ke [pastebin](http://pastebin.com/) atau screenshoot hasil debuggingnya dan upload ke [imagebam](http://www.imagebam.com/) atau [postimage](http://postimage.org/) lalu paste linknya kemari untuk dianalisa

mematikan squid

{% highlight ruby %}
net stop squid
{% endhighlight %}

mengatur agar squid membaca ulang konfigurasi tanpa perlu melakukan restart service

{% highlight ruby %}
squid -n squid -k reconfigure
{% endhighlight %}

untuk mendelete service squid yang telah diinstall (sebelumnya telah menginstall service dari paket squid ini)

{% highlight ruby %}
squid -r
{% endhighlight %}

simple tools untuk monitoring performance squid

{% highlight ruby %}
squidclient mgr:info
{% endhighlight %}

untuk mengecek website yang diakses melalui access.log yang bekerja secara realtime anda dapat mendownload tools yang bernama `mtail` di link berikut [http://ophilipp.free.fr/op_tail.htm](http://ophilipp.free.fr/op_tail.htm) atau jika ingin tampilan log dengan warna mendekati ccze silakan memakai `baretail` di link berikut [https://www.baremetalsoft.com/baretail/](https://www.baremetalsoft.com/baretail/) 

petunjuk penggunaannya simple. tinggal arahkan saja ke file access.log, namun karena default type yang dibaca adalah `.txt` maka anda harus merubahnya ke `all files`

jika ada pertanyaan lebih lanjut silakan main saja ke forum berikut [http://www.comstuff.net/showthread.php?t=12](http://www.comstuff.net/showthread.php?t=12)

**NOTE:** 
Bagi yang bermasalah dengan cache youtube, pastikan saat di tes tidak menggunakan addon **ADBLOCK PLUS** di browsernya.
Jika Menggunakan addon Adblock Plus pastikan filter yang terpasang **HANYA** `abpindo + easylist`. jika ada filter selain itu kemungkinan ga' tercache

**Tambahan Konfigurasi**
Bagi yang mengalami **PB DC** bisa ditambah konfigurasi berikut :

- untuk bagian `Port`

{% gist Rh354/9bdaa84d8d95f6d5d588 %}

- Untuk bagian `Pattern`

{% gist Rh354/033212804384f426b0cb %}

jika sudah ditambahkan dan sudah di pakai tolong di report kemari atau di thread



