---
layout: post
title: "Instalasi Dan Konfigurasi Corosync Pacemaker HA-Clustering Di OpenSUSE"
date: 2015-02-01 10:25:53 +0700
comments: true
categories: tutorial
tags: [linux, tutorial]
fullview: false
---
Corosync merupakan bagian dari pacemaker, mirip seperti heartbeat. Fungsinya adalah sebagai *loadbalancer* dan *checker* pada *cluster environment*. OpenSUSE 12.3 tidak memuat software ini pada repositori standar. Tapi bukan berarti tidak ada paket RPM yang bisa dengan mudah anda install. OpenSUSE menyediakan paket ini pada **HA:Clustering repository**.

{% highlight ruby %}
zypper ar -f http://download.opensuse.org/repositories/network:/ha-clustering:/Stable/openSUSE_12.3
zypper ref -f
zypper install pacemaker corosync
{% endhighlight %}

Setelah selesai instalasi paket - paket beserta *dependencies*, langkah selanjutnya adalah mempersiapkan [SSH passwordless]({{site.baseurl}}{% post_url 2015-01-20-login-ssh-tanpa-password %}) . Gunanya selain nantinya memudahkan untuk singkronisasi file dalam *clustering*, dapat juga mempermudah perpindahan file konfigurasi pada setiap server yang di *cluster*. Sebelumnya saya akan menjelaskan *environment* yang saya pakai pada tulisan ini.

{% highlight ruby %}
Cluster Node 1                              Cluster Node 2
OpenSUSE 12.3                               OpenSUSE 12.3
Minimal text based                          Minimal text based
Hostname : cluster-00.techshoot.org         Hostname : cluster-01.techshoot.org
IP : 192.168.184.20                         IP : 192.168.184.21
{% endhighlight %}

{% highlight ruby %}
corosync-keygen
{% endhighlight %}

Corosync menggunakan enkripsi otentikasi 1024 bit secara *default*, namun pada beberapa kasus corosync generate semua metode enkripsi. Oleh karena itu jika proses generate authkey terjadi sangat lama, biarkan saja dan tunggu hingga selesai dengan sendirinya. Setelah proses generate authkey selesai, lakukan copy authkey ke Cluster Node lainnya.

{% highlight ruby %}
scp /etc/corosync/authkey root@cluster-01:/etc/corosync/
{% endhighlight %}

Proses copy menggunakan scp kadang tidak berhasil jika anda belum melakukan [SSH passwordless]({{site.baseurl}}{% post_url 2015-01-20-login-ssh-tanpa-password %}). Setelah berhasil copy authkey, selanjutnya adalah konfigurasi corosync. Corosync pada OpenSUSE memiliki standar konfigurasi yang bisa menjadi acuan anda, terletak pada directory /etc/corosync. Saya akan menggunakan corosync.conf.example.udpu sebagai acuan konfigurasi.

{% highlight ruby %}
cp /etc/corosync/corosync.conf.example.udpu /etc/corosync/corosync.conf
{% endhighlight %}

{% highlight ruby %}
vi /etc/corosync/corosync.conf
{% endhighlight %}

Berikut contoh konfigurasi yang saya gunakan, ada penambahan konfigurasi pada baris 3 - 6 yaitu penambahan user dan group untuk menjalankan openais, pacemaker dan corosync. Selain itu pada **memberaddr**, anda harus mendeskripsikan IP node - node yang digunakan. Langkah selanjutnya periksa firewall konfigurasi pada semua Cluster Node !. Jika firewall aktif, anda harus membuka port **UDP 5405** untuk komunikasi corosync antar node.

{% gist Rh354/5fe86c70d3192d4da56c %}

Buat sebuah file baru pada `/etc/corosync/service.d/pcmk` gunanya untuk mendeskripsikan engine apa yang akan digunakan oleh corosync.

{% highlight ruby %}
vi /etc/corosync/service.d/pcmk
{% endhighlight %}

Isi dengan baris - baris berikut :

{% highlight ruby %}
service {
name: pacemaker
ver: 0
}
{% endhighlight %}

{% highlight ruby %}
scp /etc/corosync/corosync.conf root@cluster-01:/etc/corosync/
{% endhighlight %}

{% highlight ruby %}
scp /etc/corosync/service.d/pcmk root@cluster-01:/etc/corosync/service.d/
{% endhighlight %}

Pada OpenSUSE initscript corosync tidak ada pada **_/etc/init.d/corosync_**, namun ada pada **_/etc/init.d/openais_**. Oleh karena itu start service corosync menggunakan command berikut,

{% highlight ruby %}
/etc/init.d/openais start
{% endhighlight %}

{% highlight ruby %}
crm_mon -1 # command check
{% endhighlight %}

{% highlight ruby %}
============
Last updated: Tue Jan 6 07:51:20 2015
Last change: 
Current DC: NONE
0 Nodes configured, unknown expected votes
0 Resources configured.
============
Online: [ cluster-00.techshoot.org ]
------------------------------
{% endhighlight %}

Lakukan verifikasi pada corosync

{% highlight ruby %}
crm_verify -L
{% endhighlight %}

Disable stonith dan quorum storage

{% highlight ruby %}
crm configure property stonith-enabled=false
{% endhighlight %}

{% highlight ruby %}
crm configure property no-quorum-policy=ignore
{% endhighlight %}

Konfigurasi virtual IP Cluster serta lama waktu pengecekan

{% highlight ruby %}
crm configure primitive VirtualIP ocf:heartbeat:IPaddr2 \
        params ip="192.168.184.10" cidr_netmask="24" \
        op monitor interval="30s"
{% endhighlight %}

Quorum merupakan storage system dari pacemaker. Karena pada tulisan ini saya hanya menggunakan 2 buah node, quorum tidak diperlukan. Sistem storage quorum berlaku jika kita sudah memiliki 3 atau lebih node, dan salah satunya sebagai storage. Namun bukan berarti memiliki 3 node atau lebih harus memiliki quorum. Sedangkan **VirtualIP** merupakan nama yang anda berikan pada Cluster IP virtual, sehingga nantinya dari luar hanya mengakses pada Virtual IP tersebut. Pastikan saat anda menetapkan **VirtualIP** merupakan **IP baru yang berbeda dengan Node manapun**. Anda bisa melihat node mana yang sedang digunakan menggunakan command **crm_mon**. Seperti pada contoh berikut :

{% highlight ruby %}
============
Last updated: Sat Jan  10 03:43:42 2015
Last change: Sat Jan  10 03:40:53 2014 by root via cibadmin on cluster-00
Stack: openais
Current DC: cluster-00 - partition with quorum
Version: 1.1.7-61a079313275f3e9d0e85671f62c721d32ce3563
2 Nodes configured, 2 expected votes
1 Resources configured.
============

Online: [ cluster-00 cluster-01 ]

VirtualIP       (ocf::heartbeat:IPaddr2):       Started cluster-00
{% endhighlight %}

**NOTE:**
Tulisan ini merupakan rewrite dari tulisan dari [blog mahirrudin](mahirrudin.com), tujuannya agar tidak lupa nantinya dan juga atas permintaan dari penulis artikel tersebut untuk dicantumkan juga disini. 

**Sumber :**
[http://snozberry.org/blog/2012/05/02/corosync-slash-pacemaker-on-centos-6/](http://snozberry.org/blog/2012/05/02/corosync-slash-pacemaker-on-centos-6/)