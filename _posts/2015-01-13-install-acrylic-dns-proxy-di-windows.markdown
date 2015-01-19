---
layout: post
title: "Install Acrylic DNS Proxy di windows"
date: 2015-01-13 10:25:53 +0700
comments: true
categories: tutorial
tags: [windows, tutorial, dns]
fullview: false
---
Di artikel sebelumnya menjelaskan tentang instalasi **[unbound]({{site.baseurl}}{% post_url 2012-06-30-install-unbound-di-windows %})** dan **[BIND]({{site.baseurl}}{% post_url 2014-10-03-install-bind-di-windows %})** artikel kali ini membahas tentang instalasi Acrylic DNS Proxy di windows untuk tujuan personal use aja, bukan di pakai di network (untuk saat ini). instalasi ini telah diuji di Windows 7 64bit dan berjalan dengan sukses hingga artikel ini di tulis.

Berikut langkah - langkahnya :

- Download Acrylic DNS Proxy di link berikut [http://mayakron.altervista.org/wikibase/show.php?id=AcrylicHome](http://mayakron.altervista.org/wikibase/show.php?id=AcrylicHome)

- Download Acrylic DNS Proxy Monitor di link berikut [http://dev.arqendra.net/index1.php#adpm](http://dev.arqendra.net/index1.php#adpm)

- Install Acrylic DNS Proxy di tempat yang diinginkan, 

![acrylic install](http://s6.postimg.org/me8h1b1oh/acrylic.png)

di artikel ini target directory adalah 
{% highlight ruby %}
C:\Acrylic DNS Proxy
{% endhighlight %}

jika telah terinstall terinstall maka akan terbuat service baru bernama **Acrylic DNS Proxy Service**

selanjutnya mengkonfigurasi dns lokal tersebut. didalam folder instalasi terdapat beberapa file yang bisa di konfigurasi yaitu `AcrylicHosts.txt` dan `AcrylicConfiguration.ini`

untuk **AcrylicHosts** sama seperti file hosts windows pada umumnya namun menurut sumber di websitenya lebih mumpuni ketika memakai blocking ads based hosts jika di bandingkan hosts bawaan windows.

contoh blocking ads based hosts bisa di cek di beberapa link berikut

[http://winhelp2002.mvps.org/hosts.htm](http://winhelp2002.mvps.org/hosts.htm) 
[http://pgl.yoyo.org/adservers/](http://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&startdate[day]=&startdate[month]=&startdate[year]=&mimetype=plaintext)
[http://someonewhocares.org/hosts/](http://someonewhocares.org/hosts/)

untuk konfigurasinya ada di **AcrylicConfiguration.ini**, berikut konfigurasinya dipakai

{% gist Rh354/66bc386b2ce15c63f8f9 %}

kalau sudah di restart servicesnya via **services.msc**

- Ekstrak file `ADPMonitor2.zip` (Acrylic DNS Proxy Monitor)
- Klik 2x file `ADPMonitor.exe`
![dns proxy monitor](http://s6.postimg.org/xf3m6btxd/dns_proxy.png)
- agar bisa jalan di startup lakukan langkah berikut
![startup proxy](http://s6.postimg.org/5uasf2ce9/startup_dns.png)

untuk testing bisa melakukan **[dig]({{site.baseurl}}{% post_url 2012-06-30-dig-di-windows %})** dengan perintah berikut

{% highlight ruby %}
dig @127.0.0.1 kaskus.co.id
{% endhighlight %}

maka hasilnya 

{% highlight ruby %}
; <<>> DiG 9.10.1-P1 <<>> @localhost kaskus.co.id
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11235
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 5, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;kaskus.co.id.                  IN      A

;; ANSWER SECTION:
kaskus.co.id.           14803   IN      A       103.6.117.3
kaskus.co.id.           14803   IN      A       103.6.117.2

;; AUTHORITY SECTION:
id.                     170586  IN      NS      a.dns.id.
id.                     170586  IN      NS      sec3.apnic.net.
id.                     170586  IN      NS      c.dns.id.
id.                     170586  IN      NS      e.dns.id.
id.                     170586  IN      NS      b.dns.id.

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Jan 13 13:36:45 SE Asia Standard Time 2015
;; MSG SIZE  rcvd: 169
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
Non-authoritative answer:
Name:   kaskus.co.id
Address: 103.6.117.3
Name:   kaskus.co.id
Address: 103.6.117.2
{% endhighlight %}