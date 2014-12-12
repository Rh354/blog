---
layout: post
title: "Install BIND di windows"
date: 2014-10-03 10:25:53 +0700
comments: true
categories: tutorial
tags: [linux, tutorial, dns]
fullview: false
---
Sudah lama ga' nulis, ini juga sempetin waktu sejenak buat nulis..hehe..
oke deh jika sebelumnya sudah di post artikel tentang **[unbound]({% post_url 2012-06-30-install-unbound-di-windows %})** artikel kali ini membahas tentang instalasi BIND di windows untuk tujuan personal use aja, bukan di pakai di network (untuk saat ini). instalasi ini telah diuji di Windows 7 64bit dan berjalan dengan sukses hingga artikel ini di tulis.

Berikut langkah - langkahnya :

- Download dahulu BIND yang terbaru di link berikut [http://www.isc.org/downloads/](http://www.isc.org/downloads/)

- Ekstrak di mana saja terlebih dahulu.

- Masuk ke folder bind dan jalankan **BINDInstall atau BINDInstall.exe**

- Maka nanti akan muncul gambar berikut

![BINDinstall](http://s6.postimg.org/8vzn8g4sh/bind.png)

di artikel ini target directory adalah 
{% highlight ruby %}
C:\BIND
{% endhighlight %}
untuk service account name  diatas bisa diganti sesuka hati namun di artikel ini dibuat default yaitu **named** selebihnya dikosongin saja dulu karena ada langkah selanjutnya

- klik install jika telah yakin

jika telah terinstall terinstall maka akan terbuat service baru bernama **ISC BIND**

jika sudah edit environment variables anda untuk memasukkan path nya BIND **(cara ini sangat penting untuk dilakukan karena akan mempengaruhi langkah selanjutnya)**. caranya klik kanan `my computer atau computer` di start menu lalu klik `properties` klik `advanced system settings` dan klik `environment variables` dan pada `system variables` cari variable `path` lalu isi seperti berikut

{% highlight ruby %}
;C:\BIND\bin
{% endhighlight %}

jika sudah di **OK** saja.

buat konfigurasi untuk BIND, berikut konfigurasi yang saya pakai

{% gist Rh354/2f23e6d49ab10f7b0707 %}

selanjutnya buka command prompt dan ketik 

{% highlight ruby %}
rndc-confgen -a
{% endhighlight %}

maka di folder `C:\BIND\etc` akan muncul file baru dengan nama **rndc.key**

lalu ketik lagi 

{% highlight ruby %}
rndc-confgen > C:\BIND\etc\rndc.conf
{% endhighlight %}

maka di folder `C:\BIND\etc` akan muncul file baru dengan nama **rndc.conf**

berikut contoh dari **rndc.conf** yang saya generate

{% gist Rh354/130e4dc6f4238ab936e2 %}

kopi beberapa baris dari **rndc.conf** yang telah di generate sebelumnya ke **named.conf**

{% highlight ruby %}
key "rndc-key" {
	algorithm hmac-md5;
	secret "CeN9Dvb6+ECaib7JL4Fb+Q==";
 };
 
 controls {
 	inet 127.0.0.1 port 953
 		allow { 127.0.0.1; } keys { "rndc-key"; };
 };
{% endhighlight %}

sehingga menjadi

{% gist Rh354/3631bda17b315bbf75e9 %}

**note:** forwarder diatas adalah forwarder google dan opendns silakan pakai forwarder lain yang di kehendaki

selanjutnya buat file **resolv.conf** di folder `C:\BIND\etc` dengan isi sebagai berikut

{% highlight ruby %}
nameserver 127.0.0.1
{% endhighlight %}

- Buka Command Prompt atau tekan logo windows dan R untuk menampilkan menu RUN lalu ketik

{% highlight ruby %}
services.msc
{% endhighlight %}

cari service **ISC BIND** tadi lalu di klik 2x

lalu pastikan diubah seperti gambar berikut lalu di ok

![ISC BIND](http://s6.postimg.org/qajvgq1xd/isc_bind.png)

langkah selanjutnya bisa melakukan start service bind tersebut.

jika lewat command prompt silakan jalankan perintah berikut untuk melakukan start

{% highlight ruby %}
net start named
{% endhighlight %}

untuk restart konfigurasi tanpa melakukan restart bind silakan ketikan perintah berikut melalui command prompt

{% highlight ruby %}
rndc reload
{% endhighlight %}

untuk restart BIND silakan ketikan perintah berikut via command prompt

{% highlight ruby %}
net stop named && net start named
{% endhighlight %}

masukkan IP BIND anda di Windows koneksi anda sebagai contoh di koneksi LAN

![LAN](http://s6.postimg.org/qol7gbm0x/dns_internet.png)

atau jika mempunyai squid bisa via squidnya

untuk testing BIND bisa melakukan dig dengan perintah berikut

{% highlight ruby %}
dig @127.0.0.1 kaskus.co.id
{% endhighlight %}

maka hasilnya sebagai berikut

{% highlight ruby %}
; <<>> DiG 9.10.1 <<>> @127.0.0.1 kaskus.co.id
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8269
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;kaskus.co.id.                  IN      A

;; ANSWER SECTION:
kaskus.co.id.           19024   IN      A       103.6.117.3
kaskus.co.id.           19024   IN      A       103.6.117.2

;; Query time: 323 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Fri Oct 03 11:33:13 SE Asia Standard Time 2014
;; MSG SIZE  rcvd: 73
{% endhighlight %}

{% highlight ruby %}
dig @localhost +bufsize=1200 +norec NS . @a.root-servers.net
{% endhighlight %}

maka hasilnya

{% highlight ruby %}
; <<>> DiG 9.10.1 <<>> @localhost +bufsize=1200 +norec NS . @a.root-servers.net
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 9539
;; flags: qr ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;.                              IN      NS

;; ANSWER SECTION:
.                       18518   IN      NS      k.root-servers.net.
.                       18518   IN      NS      a.root-servers.net.
.                       18518   IN      NS      d.root-servers.net.
.                       18518   IN      NS      j.root-servers.net.
.                       18518   IN      NS      i.root-servers.net.
.                       18518   IN      NS      g.root-servers.net.
.                       18518   IN      NS      b.root-servers.net.
.                       18518   IN      NS      e.root-servers.net.
.                       18518   IN      NS      f.root-servers.net.
.                       18518   IN      NS      h.root-servers.net.
.                       18518   IN      NS      m.root-servers.net.
.                       18518   IN      NS      l.root-servers.net.
.                       18518   IN      NS      c.root-servers.net.

;; Query time: 1 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Fri Oct 03 11:38:52 SE Asia Standard Time 2014
;; MSG SIZE  rcvd: 239
{% endhighlight %}

{% highlight ruby %}
dig +bufsize=1200 +norec NS . @localhost
{% endhighlight %}

{% highlight ruby %}
; <<>> DiG 9.10.1 <<>> +bufsize=1200 +norec NS . @localhost
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15715
;; flags: qr ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;.                              IN      NS

;; ANSWER SECTION:
.                       18347   IN      NS      l.root-servers.net.
.                       18347   IN      NS      e.root-servers.net.
.                       18347   IN      NS      f.root-servers.net.
.                       18347   IN      NS      g.root-servers.net.
.                       18347   IN      NS      c.root-servers.net.
.                       18347   IN      NS      d.root-servers.net.
.                       18347   IN      NS      k.root-servers.net.
.                       18347   IN      NS      m.root-servers.net.
.                       18347   IN      NS      h.root-servers.net.
.                       18347   IN      NS      i.root-servers.net.
.                       18347   IN      NS      b.root-servers.net.
.                       18347   IN      NS      j.root-servers.net.
.                       18347   IN      NS      a.root-servers.net.

;; ADDITIONAL SECTION:
a.root-servers.net.     17955   IN      A       198.41.0.4

;; Query time: 1 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Fri Oct 03 11:41:43 SE Asia Standard Time 2014
;; MSG SIZE  rcvd: 255
{% endhighlight %}

untuk tes via nslookup melalui command prompt langkahnya sebagai berikut

{% highlight ruby %}
nslookup
{% endhighlight %}
 
via name server mana ?
{% highlight ruby %}
server 127.0.0.1
{% endhighlight %}
 
nama web yang mau di lookup
{% highlight ruby %}
kaskus.co.id
{% endhighlight %}
 
 maka hasilnya adalah

 {% highlight ruby %} 
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   kaskus.co.id
Address: 103.6.117.3
Name:   kaskus.co.id
Address: 103.6.117.2
{% endhighlight %}