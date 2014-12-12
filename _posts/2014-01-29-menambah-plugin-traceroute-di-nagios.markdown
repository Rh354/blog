---
layout: post
title: "menambah plugin traceroute di nagios"
date: 2014-01-29 17:36:55 +0700
comments: true
categories: tutorial
tags: [linux, tutorial, nagios]
fullview: false 
---
artikel ini bermula dikarenakan gara - gara koneksi internet di kantor mati tiba - tiba jadilah akhirnya mencari cara agar ketika koneksi mati nagios langsung kasih laporan, tujuannya sich agar ketahuan mati berapa lama.

setelah browsing sana sini dengan teman - teman kantor akhirnya ketemulah plugin dengan command yang simple di nagios. di tutorial ini akan kita skip langkah instalasi di nagios dan lain - lainnya.

di artikel ini plugin tersebut adalah `check_traceroute`

save as lah code di bawah ini dengan nama check_traceroute di folder nagios plugin anda. di artikel ini kebetulan ada di `/usr/lib/nagios/plugins/`

{% gist Rh354/859ab39984d7b381ca83 %}

jika sudah chmod terlebih dahulu plugin tersebut

{% highlight ruby %}
chmod +x /usr/lib/nagios/plugins/check_traceroute
{% endhighlight %}

Jika mengalami error tentang `/usr/bin/gethostip` (error yang terjadi ketika artikel ini dibuat dengan menggunakan opensuse) maka silakan menginstall paket `syslinux` via **yast** ataupun **zypper** 

di config service nagios anda silakan tambahkan

{% highlight ruby %}
# Define a service to check Connection eth0 on the local machine to internet.
define service{
        use                             local-service         ; Name of service template to use
        host_name                       nagios
        service_description             Internet Fastnet #Penamaan untuk koneksi anda
        check_command                   check_traceroute!8.8.8.8 #8.8.8.8 adalah ip tujuan traceroute
        }
{% endhighlight %}

lalu restart nagios anda

{% highlight ruby %}
service nagios restart
{% endhighlight %}