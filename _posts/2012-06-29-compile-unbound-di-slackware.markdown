---
layout: post
title: "Compile Unbound di Slackware"
date: 2012-06-29 18:55
comments: true
categories: tutorial
tags: [tutorial, linux, dns]
fullview: false
---

>**[Unbound](http://www.unbound.net/)** is a validating, recursive, and caching DNS resolver.

begitulah kata iklan di websitenya tapi setelah gw compile dan coba dengan trial dan error ternyata lebih responsif dari dns yang dah gw pake sebelumnya yaitu **[DNSMASQ](http://www.thekelleys.org.uk/dnsmasq/doc.html)**, **[PDNSD](http://www.phys.uu.nl/~rombouts/pdnsd/index.html)** (berdasarkan feeling dan graphic dibawah ini).

![unbound_graph](http://s6.postimage.org/pp30gyy0x/2epkmr5.png)

ok sekarang langsung saja gw mencoba di laptop axioo butut kesayangan gw yang terinstal slackware 13.1


masuk dengan mode root dulu

{% highlight ruby %}
# cd /tmp
{% endhighlight %}

download unbound terbaru

{% highlight ruby %}
# wget http://www.unbound.net/downloads/unbound-latest.tar.gz
{% endhighlight %}

hingga artikel ini diluncurkan gw memakai **unbound 1.4.17**

{% highlight ruby %}
# tar xvf unbound-latest.tar.gz
# cd unbound-1.4.6

# ./configure
# make
# make install
{% endhighlight %}

add grup untuk unbound

{% highlight ruby %}
# groupadd unbound
# useradd -d /var/unbound -m -g unbound -s /bin/false unbound
{% endhighlight %}

konfigurasi unbound.conf nya dengan menggunakan vi,kwrite ato editor kesayangan kalian(dalam hal ini gw make vi ). 
tuk versi compile lokasinya ada dimari **/usr/local/etc/unbound/unbound.conf**

{% highlight ruby %}
# vi /usr/local/etc/unbound/unbound.conf
{% endhighlight %}

ini settingan gw

{% gist Rh354/f44efa1ac14dbacbbaaf %}

silahkan ganti dns.indolini.org dengan domain laen & silahkan sesuaikan ip nya dengan networknya.

cek konfigurasi dengan perintah ini
{% highlight ruby %}
# unbound-checkconf /usr/local/etc/unbound/unbound.conf
{% endhighlight %}

klo ga' ada error berarti aman

download file **named.cache** nya

{% highlight ruby %}
# cd /usr/local/etc/unbound/unbound
# wget  ftp://FTP.INTERNIC.NET/domain/named.cache
{% endhighlight %}

klo udah sekarang merequest file key server agar unbound dapat berjalan

{% highlight ruby %}
# cd /usr/local/sbin
# unbound-control-setup
# cd /usr/local/etc/unbound
# chown unbound:root unbound_*
# chmod 440 unbound_*
{% endhighlight %}

lalu buat skrip agar unbound bisa dieksekusi

{% highlight ruby %}
# vi /etc/rc.d/rc.unbound
{% endhighlight %}

lalu isi dengan skrip dibawah ini

{% gist Rh354/40cc047dce2b6f9fb421 %}

save lalu buat menjadi executable

{% highlight ruby %}
# chmod 755 /etc/rc.d/rc.unbound
{% endhighlight %}

atau

{% highlight ruby %}
# chmod +x /etc/rc.d/rc.unbound
{% endhighlight %}

klo udah mari kita buat startup skripnya
{% highlight ruby %}
# vi /etc/rc.d/rc.local
{% endhighlight %}

masukkan skrip ini tapi klo ada squid masukkan sebelum skrip auto start squid (klo ada squid, klo ga' ada file nya silahkan buat aja )

{% gist Rh354/d5c38cc2df87c214457c %}

untuk shutdown silahkan memakai skrip ini di local_shutdown script setelah auto shutdown squid(klo ada squidnya klo ga' ada masukkan langsung aja, klo filenya ga'; ada silahkan buat aja )

{% highlight ruby %}
# vi /etc/rc.d/rc.local_shutdown
{% endhighlight %}

{% gist Rh354/7427ffa5a792e27d2755 %}

sekarang coba di restart unbound nya

{% highlight ruby %}
# /etc/rc.d/rc.unbound restart
{% endhighlight %}

asumsi memakai dns.indolini.org di penamaan domainnya

{% highlight ruby %}
# root@indolini:~# nslookup 192.168.xx.xx
# Server: 127.0.0.1
# Address: 127.0.0.1#53

# x.xx.168.192.in-addr.arpa name = dns.indolini.org.
{% endhighlight %}

{% highlight ruby %}
# root@indolini:~# nslookup dns.indolini.org
# Server: 127.0.0.1
# Address: 127.0.0.1#53

# Name: dns.indolini.org
# Address: 192.168.xx.xx
{% endhighlight %}

masukkan dns nya ke squid lalu di reconfigure

{% highlight ruby %}
dns_nameservers 127.0.0.1
{% endhighlight %}

untuk melihat performanya silahkan cek dengan ini

{% highlight ruby %}
# unbound-control stats
{% endhighlight %}

tolong diberitahu yah apabila nubi ini ada kesalahan,maklum cuma modal nekat aja trial & error selama kurang lebih 1 hari dengan kemampuan yang terbatas

