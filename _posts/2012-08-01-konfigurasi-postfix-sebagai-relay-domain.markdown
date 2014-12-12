---
layout: post
title: "Konfigurasi Postfix sebagai Relay Domain"
date: 2012-08-01 11:19
comments: true
fullview: false
categories: tutorial
tags: [Tutorial, Linux, postfix]
---

Berawal dari coba - coba membuat mail relay dengan postfix akhirnya nubi memberanikan diri untuk di posting kesini agar tidak lupa dengan pelajaran berharga yang udah didapat. Bagi yang belum tau tentang postfix, berikut ulasan mengenai postfix

> Postfix adalah sebuah program pengirim email yang ditulis oleh Wietse Venema, yang mulai menjadi alternatif lain terhadap dominasi penggunaan Sendmail. 

Postfix berusaha menjadi program yang cepat, mudah dikelola, dan aman, dimana juga harus cukup sesuai dan cocok dengan Sendmail sehingga tidak mengecewakan penggunanya. Maka dari itu, jika dilihat sekilas dari luar nampak mirip seperti Sendmail, tapi didalam semuanya berbeda.

Anggap postfix sudah terinstall dan jalan dengan sempurna di CPU anda, sekarang kita akan melakukan relay berdasarkan domain, keuntungannya adalah dari sisi relay tidak perlu mailbox user, jadi mailbox tetap di mail utama yang akan di relay.

Langkah pertama adalah buat file baru dengan nama terserah di dalam **/etc/postfix** dalam contoh ini gw buat dengan nama **relaydomain**

{% highlight ruby %}
$ sudo vi /etc/postfix/relaydomain
{% endhighlight %}

isi seperti berikut

{% highlight ruby %}
domain a.com OK
domain b.com OK
{% endhighlight %}

lalu buat file transport

{% highlight ruby %}
$ sudo vi /etc/postfix/transport
{% endhighlight %}

lalu isi filenya sebagai berikut

{% highlight ruby %}
domain a.com smtp:[ip server mail utama a]
domain b.com smtp:[ip server mail utama b]
{% endhighlight %}

jika sudah silakan lakukan postmap dulu agar db nya terbentuk

{% highlight ruby %}
$ sudo postmap /etc/postfix/relaydomain
$ sudo postmap /etc/postfix/transport
{% endhighlight %}

langkah selanjutnya adalah mengedit settingan postfixnya

{% highlight ruby %}
$ sudo vi /etc/postfix/main.cf
{% endhighlight %}

dan silakan tambahkan baris berikut di paling bawah settingan postfix anda di **main.cf**

{% highlight ruby %}
relay_domains = $mydomain, /etc/postfix/relaydomain
transport_maps = hash:/etc/postfix/transport
{% endhighlight %}

secara keseluruhan isi config main.cf nya seperti ini

{% gist Rh354/a97d2653be1e7a39fa1e %}

**NOTE:** ip yang ditulis di relay host adalah ip mail utama. 

lalu reload postfix anda agar dapat membaca settingan postfix yang baru

{% highlight ruby %}
$ sudo postfix reload
{% endhighlight %}

jika sudah silakan allow di mail server utama agar dapat menerima mail dari postfix relay (trusted ip)

**NOTE :**
Dalam postfix yang gw buat terdapat case bahwa si postfix tidak bisa mengirim email relay (dari user yang mengirim lewat relay) ke mail utama (rejected). 

Hal ini teratasi dengan memodifikasi sedikit config sehingga menjadi berubah menjadi seperti ini.

{% highlight ruby %}
smtpd_recipient_restrictions = permit_mynetworks, reject_unknown_recipient_domain, reject_unknown_sender_domain, reject_rbl_client cbl.abuseat.org, reject_rbl_client sbl-xbl.spamhaus.org, reject_rbl_client zen.spamhaus.org, permit, check_relay_domains
{% endhighlight %}

untuk mengenal lebih jauh tentang postfix silakan mampir ke webnya postfix indonesia di link berikut [Postfix Indonesia](http://www.postfix.or.id/)

Source : [milist linux](http://www.mail-archive.com/tanya-jawab@linux.or.id/msg72623.html)