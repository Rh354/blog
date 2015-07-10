---
layout: post
title: "Install Squid 3 di Windows"
date: 2015-07-10 10:25:53 +0700
comments: true
categories: tutorial
tags: [windows,squid,tutorial]
fullview: false
---
Mungkin beberapa netizen sudah tidak asing lagi dengan program squid dan kemampuannya, namun bagi yang masih baru mengenal squid silakan di lihat lihat terlebih dahulu melalui link berikut [http://www.squid-cache.org/](http://www.squid-cache.org/)

Tutorial ini dibuat untuk keperluan pribadi dengan seadanya saja jadi mohon di maafkan jika ada kesalahan dalam pembuatan. Untuk di windows sendiri saya menggunakan squid 3 yang di developed oleh pihak lain yaitu [diladele](http://squid.diladele.com/) dan squid yang dipakai saat tutorial ini di kerjakan adalah **Squid 3.5.6**

Langkah - langkahnya adalah :

1. Download squid 3 dari link berikut [http://squid.diladele.com/](http://squid.diladele.com/)
2. Download file ssl dan sertifikat yang nantinya akan di gunakan di squid melalui link berikut [https://db.tt/6dZ3Oy65](https://db.tt/6dZ3Oy65)
3. Install seperti biasa msi file squid tadi. (kalau sudah terinstall nanti akan muncul shortcut squid tray di desktop)
4. extract file zip ssl dan sertifikat yang telah di download sebelumnya maka akan ada 2 file yaitu **myCA.der** dan **myCA.pem** dan ada di dalam folder `ssl_cert`. 
5. Kopi folder tersebut ke dalam `C:\squid\etc\squid` sehingga menjadi `C:\squid\etc\squid\ssl_cert`
6. kopi dan paste squid.conf di bawah ini

{% gist Rh354/dcd73711f50fdfbf742b %}

di konfigurasi squid diatas ada beberapa hal yang **harus di perhatikan**

- karena squid ini dicompile via cygwin maka penulisannya sedikit berbeda. seharusnya `D:/squid3cache` menjadi `/cygdrive/d/squid3cache`
- jika ingin diubah drivenya silakan cek cache squid, coredump, serta log. (silakan sesuaikan)
- 5000 di bagian cache maksudnya adalah 5Gb silakan jika ingin disesuaikan dan mungkin link [cache dir di squid]({{site.baseurl}}{% post_url 2012-06-30-cache-dir-di-squid %}) dapat membantu
- dns yang dipakai dalam tutorial ini adalah **DNS NAWALA** disarankan jika ingin mengubah bisa memakai dns resolver seperti [acrylic dns]({{site.baseurl}}{% post_url 2015-01-13-install-acrylic-dns-proxy-di-windows %}) atau [unbound dns]({{site.baseurl}}{% post_url 2012-06-30-install-unbound-di-windows %}) lalu ubah konfig dns_nameservers nawala menjadi `dns_nameservers 127.0.0.1`
- konfigurasi diatas tidak mengikut sertakan **store-id.pl** karena ketika dicoba squidnya crash, mungkin ada yang salah. mungkin kalau ada yang berhasil bisa share caranya disini.

jika sudah edit environment variables anda untuk memasukkan path nya squid **(cara ini sangat penting untuk dilakukan karena akan mempengaruhi langkah selanjutnya)**. caranya klik kanan `my computer atau computer` di start menu lalu klik `properties` klik `advanced system settings` dan klik `environment variables` dan pada `system variables` cari variable `path` lalu isi seperti berikut

{% highlight ruby %}
;C:\squid\bin;C:\Squid\lib\squid
{% endhighlight %}

langkah selanjutnya membuat data base untuk `SSL`

- buat folder baru di `C:\squid\var` dengan nama folder **ssl_db**

buka CMD dan ketik perintah berikut untuk 

{% highlight ruby %}
C:\Squid\lib\squid\ssl_crtd.exe -c -s C:\Squid\var\ssl_db\certs
{% endhighlight %}

Klik kanan - properties folder instalasi squid dan pastikan permission seluruh user di bagian security untuk folder tersebut read write atau full control.

lalu untuk membuat directory squid maka ketik command berikut

{% highlight ruby %}
squid -z
{% endhighlight %}

cek dengan perintah berikut dan nanti akan di beri cluenya yang error dimana

{% highlight ruby %}
squid -k parse
{% endhighlight %}

restart service squid dengan cara klik kanan icon tray squid lalu klik top dan lakukan start kembali.

edit proxy browser

![proxy browser](http://s6.postimg.org/bqy7nj8ox/browser_proxy.png)

agar bisa berjalan di **HTTPS** silakan import sertifikat file **myCA.der** yang ada di folder `C:\Squid\etc\ssl_cert`

untuk cara import ada di link berikut

**Firefox** : [https://wiki.wmtransfer.com/projects/webmoney/wiki/Installing_root_certificate_in_Mozilla_Firefox](https://wiki.wmtransfer.com/projects/webmoney/wiki/Installing_root_certificate_in_Mozilla_Firefox)

**chrome**	: [https://wiki.wmtransfer.com/projects/webmoney/wiki/Installing_root_certificate_in_Google_Chrome](https://wiki.wmtransfer.com/projects/webmoney/wiki/Installing_root_certificate_in_Google_Chrome)

lalu tes buka link https seharusnya verifiednya udah berubah seperti contoh berikut

![sertifikat browser](http://s6.postimg.org/ovzhmwhmp/verified.png)

untuk mengecek website yang diakses melalui access.log yang bekerja secara realtime anda dapat mendownload tools yang bernama `mtail` di link berikut [http://ophilipp.free.fr/op_tail.htm](http://ophilipp.free.fr/op_tail.htm) atau jika ingin tampilan log dengan warna mendekati ccze silakan memakai `baretail` di link berikut [https://www.baremetalsoft.com/baretail/](https://www.baremetalsoft.com/baretail/) 

petunjuk penggunaannya simple. tinggal arahkan saja ke file access.log, namun karena default type yang dibaca adalah `.txt` maka anda harus merubahnya ke `all files`

contoh hit facebook dengan konfigurasi di atas

**TCP_MEM_HIT**
![tcp mem hit](http://s6.postimg.org/dd4loaaf5/tcp_mem_hit.png)

**TCP_HIT**
![tcp hit](http://s6.postimg.org/9uslrw9j5/tcp_hit.png)

