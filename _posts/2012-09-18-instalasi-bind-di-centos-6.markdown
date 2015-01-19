---
layout: post
title: "Instalasi BIND Chroot di CentOS"
date: 2012-09-18 09:58
comments: true
fullview: false
categories: tutorial
tags: [linux,tutorial,DNS]
---

Sebelum masuk ke tahap instalasi ada beberapa hal yang mesti di tetapkan terlebih dahulu.

* Nama Domain adalah indolini.org
* DNS yang akan dibuat adalah ns1.indolini.org dan ns2.indolini.org
* mail yang digunakan adalah mail.indolini.org
* mempunyai subdomain rhesa.indolini.org


dan berikut ip publik nya

* indolini.org dengan ip 192.168.56.5
* ns1 dengan ip 192.168.56.5
* ns2 dengan ip 192.168.56.6
* mail.indolini.org dengan ip 192.168.56.6
* rhesa.indolini.org dengan ip 192.168.56.5

Berikut langkah - langkah instalasi Bind Chroot di centos (kebetulan yang dipakai disini adalah 6.3).

bind yang di pakai di sini adalah bind 9.8.2

{% highlight ruby %}
yum install bind bind-chroot
{% endhighlight %}

sebelumnya kita kopi dahulu file yang dibutuhkan

{% highlight ruby %}
cp -R /usr/share/doc/bind-9.8.2/ /var/named/chroot/var/named/
{% endhighlight %}

{% highlight ruby %}
touch /var/named/chroot/var/named/data/cache_dump.db
{% endhighlight %}

{% highlight ruby %}
touch /var/named/chroot/var/named/data/named_stats.txt
{% endhighlight %}

{% highlight ruby %}
touch /var/named/chroot/var/named/data/named_mem_stats.txt
{% endhighlight %}

{% highlight ruby %}
touch /var/named/chroot/var/named/data/named.run
{% endhighlight %}

{% highlight ruby %}
mkdir /var/named/chroot/var/named/dynamic
{% endhighlight %}

{% highlight ruby %}
touch /var/named/chroot/var/named/dynamic/managed-keys.bind
{% endhighlight %}

{% highlight ruby %}
chmod -R 777 /var/named/chroot/var/named/data
{% endhighlight %}

{% highlight ruby %}
chmod -R 777 /var/named/chroot/var/named/dynamic
{% endhighlight %}

set ini jika tidak ingin memakai ipv6

{% highlight ruby %}
echo 'OPTIONS="-4"' >> /etc/sysconfig/named
{% endhighlight %}

lalu generate key rndc

{% highlight ruby %}
rndc-confgen -a -c /etc/rndc.key
{% endhighlight %}

edit file named.conf di folder **/var/named/chroot/etc**

{% highlight ruby %}
nano /var/named/chroot/etc/named.conf
{% endhighlight %}

menjadi seperti berikut

{% gist Rh354/7579a542c957d6b42b4b %}

lalu buat zonenya

{% highlight ruby %}
cd /var/named/chroot/var/named 
{% endhighlight %}

{% highlight ruby %}
cp named.localhost indolini.org.zone
{% endhighlight %}

{% highlight ruby %}
nano indolini.org.zone
{% endhighlight %}

dan ubah isinya

{% gist Rh354/efcc6a0003b5c73b4987 %}

copy lagi isinya menjadi seperti ini

{% highlight ruby %}
cp indolini.org.zone 192.168.56.zone
{% endhighlight %}

{% highlight ruby %}
nano 192.168.56.zone
{% endhighlight %}

{% gist Rh354/127b9cda5574934127d0 %}

lakukan start bind

{% highlight ruby %}
/etc/init.d/named start
{% endhighlight %}

masukkan 127.0.0.1 ke **/etc/resolv.conf**

{% highlight ruby %}
nano /etc/resolv.conf
{% endhighlight %}

{% highlight ruby %}
nameserver 127.0.0.1
{% endhighlight %}

jika sudah start

lakukan dig untuk memastikan sudah berjalan lancar atau belum

{% highlight ruby %}
dig indolini.org
{% endhighlight %}

{% highlight ruby %}
; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.10.rc1.el6_3.3 <<>> indolini.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 247
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 2, ADDITIONAL: 2

;; QUESTION SECTION:
;indolini.org.                      IN      A

;; ANSWER SECTION:
indolini.org.               3600    IN      A       192.168.56.5

;; AUTHORITY SECTION:
indolini.org.               3600    IN      NS      ns2.indolini.org.
indolini.org.               3600    IN      NS      ns1.indolini.org.

;; ADDITIONAL SECTION:
ns1.indolini.org.           3600    IN      A       192.168.56.5
ns2.indolini.org.           3600    IN      A       192.168.56.6

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Mon Sep 24 23:29:13 2012
;; MSG SIZE  rcvd: 110
{% endhighlight %}

{% highlight ruby %}
nslookup indolini.org
Server:         127.0.0.1
Address:        127.0.0.1#53

Name:   indolini.org
Address: 192.168.56.5
{% endhighlight %}

{% highlight ruby %}
 nslookup 192.168.56.5
Server:         127.0.0.1
Address:        127.0.0.1#53

5.56.168.192.in-addr.arpa        name = indolini.org.
{% endhighlight %}

dan buat agar bind berjalan di startup

{% highlight ruby %}
chkconfig bind on
{% endhighlight %}

**NOTE :**
Kesalahan umum yang sering terjadi di bind adalah :

1. service tidak bisa di start karena file tidak ditemukan (not found) solusinya karena permision untuk ownernya tidak ada untuk di centos pastikan file yang not found tersebut berada di grup yang sama cek dengan perintah ls -la /var/named/chroot/var/named jika grupnya masih root maka disamakan seperti yang lain.
 
{% highlight ruby %}
chown -R root:named /var/named/chroot/var/named
{% endhighlight %}

2. sudah di chown tapi masih ga' bisa di start. coba cek file zonenya (di gw indolini.org.zone). pastikan penempatannya bener. kurang satu titik udah pasti ngambek si bind. 

Referensi lebih lanjut silakan baca link berikut
[ini](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch18_%3a_Configuring_DNS)