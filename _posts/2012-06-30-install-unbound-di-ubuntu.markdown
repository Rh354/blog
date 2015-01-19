---
layout: post
title: "Install Unbound Di Ubuntu"
date: 2012-06-30 14:42
comments: true
categories: tutorial
tags: [linux, tutorial, dns]
fullview: false
---


Bagi kalangan para squider mungkin dah ga' asing lagi dengan nama **[PDNSD](http://www.phys.uu.nl/~rombouts/pdnsd/index.html)**, **[BIND](http://www.bind9.net/)**, **[DNSMASQ](http://www.thekelleys.org.uk/dnsmasq/doc.html)** dll yang gunanya sebagai dns resolver. Kali ini  gw akan mencoba menggunakan unbound sebagai pengganti DNS resolver diatas

sebelum melangkah lebih jauh silahkan ditengok graphic dibawah ini
![Unbound](http://s6.postimage.org/pp30gyy0x/2epkmr5.png)

okey langsung saja dimulai tahap instalasinya di ubuntu.cukup simpel koq


{% highlight ruby %}
$ sudo apt-get install unbound
{% endhighlight %}

klo udah silahkan lakukan konfigurasi file dibawah ini :

{% highlight ruby %}
$ cd /etc/unbound
{% endhighlight %}

{% highlight ruby %}
$ sudo wget  ftp://FTP.INTERNIC.NET/domain/named.cache
{% endhighlight %}

{% highlight ruby %}
$ sudo unbound-control-setup
$ sudo chown unbound:root unbound_*
$ sudo chmod 440 unbound_*
{% endhighlight %}

<p>sesuaikan config **/etc/unbound/unbound.conf**, dan servis dns lainnya **(bind/dnsmasq dll)** harus di **stop** agar tidak bentrok)

sekarang kita konfigurasi isi unboundnya. silahkan disesuaikan bagi yang mencobanya

{% highlight ruby %}
$ sudo vi /etc/unbound/unbound.conf
{% endhighlight %}

{% gist Rh354/9fe0f4e0143f5ee286b7 %}

klo udah silahkan cek filenya dl siapa tau ada yang error dengan perintah

{% highlight ruby %}
$ sudo unbound-checkconf /etc/unbound/unbound.conf
{% endhighlight %}

yang gw kasih tanda pagar silahkan sesuaikan dengan ip(yg ada **xx**nya) dan zonenya masing2.
untuk modem ato yang pake dhcp silahkan dipagar aja di depan masing2 kalimat yang gw bold diatas

klo udah silahkan restart unboundnya
{% highlight ruby %}
$ sudo /etc/init.d/unbound restart
{% endhighlight %}

sekarang tes (asumsi dah jalan)

{% highlight ruby %}
root@indolini:~$ nslookup 192.168.xx.xx
Server: 127.0.0.1
Address: 127.0.0.1#53

x.xx.168.192.in-addr.arpa name = dns.indolini.org.
{% endhighlight %}

{% highlight ruby %}
root@indolini:~$ nslookup dns.indolini.org
Server: 127.0.0.1
Address: 127.0.0.1#53

Name: dns.indolini.org
Address: 192.168.xx.xx
{% endhighlight %}

klo udah silahkan tambahkan dns localhost di squid.conf nya

{% highlight ruby %}
dns_nameservers 127.0.0.1
{% endhighlight %}

lalu rekonfigurasi ulang squidnya (dah tau jg khan perintahnya )
untuk melihat performanya silahkan di cek dengan perintah ini

{% highlight ruby %}
$ sudo unbound-control stats
{% endhighlight %}

klo ada kesalahan mohon maaf yah soalnya nubi baru mempelajari unbound selama 1 hari itupun dengan modal nekat trial dan error

untuk slackware bisa di link [berikut]({{site.baseurl}}{% post_url 2012-06-29-compile-unbound-di-slackware %})