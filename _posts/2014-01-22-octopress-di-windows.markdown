---
layout: post
title: "octopress di windows"
date: 2014-01-22 11:12:30 +0700
comments: true 
fullview: false
categories: tutorial
tags: [tutorial,windows, octopress]
---
sebelum melakukan setup octopress di windows alangkah baiknya jika menginstall python terlebih dahulu. untuk instalasinya dapat di cek melalui link berikut **[ini]({% post_url 2014-01-22-install-python-di-windows-7 %})**. ini dilakukan buat meminimalisir error tentang python nantinya.

jika sudah maka kita perlu menyiapkan alat - alat perang lainnya, yaitu :


1. install terlebih dahulu **[git](http://git-scm.com/)**
2. install **[Ruby installer untuk windows](http://rubyinstaller.org/downloads/)**, di artikel ini memakai installer **[Ruby 1.9.3-p194](http://rubyforge.org/frs/download.php/76054/rubyinstaller-1.9.3-p194.exe)**
3. install **[Development Kit](http://rubyinstaller.org/downloads/)** di artikel ini menggunakan **[DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe ](DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)** lalu ekstrak ke folder yang anda kehendaki. di tutorial ini di <code>ekstrak ke C:/RubyDevKit</code>
4. buat folder di <code>drive C</code> dengan nama <code>github</code> **(di dalam artikel ini drive c folder github tersebut hanya sebagai contoh, silakan di kreasikan sendiri)**

langkah selanjutnya adalah setup octopress melalui command prompt

{% highlight ruby %}cd C:/github
git clone git://github.com/imathis/octopress.git techshoot #ganti techshoot menjadi username.github.com
cd techshoot #ganti techshoot menjadi username.github.com
ruby --version # seharusnya terlihat Ruby 1.9.3{% endhighlight %}

lalu ikuti langkah berikut

{% highlight ruby %}cd C:/RubyDevKit
ruby dk.rb init
ruby dk.rb install{% endhighlight %}

selanjutnya install file - file pendukungnya

{% highlight ruby %}cd C:/github/octopress #replace octopress with username.github.com
gem install bundler
bundle install{% endhighlight %}

install default tema octopress
{% highlight ruby %}rake install{% endhighlight %}

Octopress telah berhasil diinstall namun belum selesai di setup, anda harus konfigurasi file _config.yml dan lebih jelasnya anda bisa membaca di [Configuring Octopress](http://octopress.org/docs/configuring/). Berikut contoh sederhana confignya.

{% highlight ruby %}
url: http://techshoot.org/
title: techshoot Blog
subtitle: cuma coretan iseng
author: techshoot
simple_search: http://google.com/search
description: Tips and Trik, tutorial
{% endhighlight %}

nah jika sudah selesai hasil generatenya nanti yang berupa **.html** di folder **public** dan bisa langsung diupload ke hostingan masing - masing atau bisa juga di upload ke github sebagai free hosting, cara setup dan uploadnya dapat dilihat di link [upload octopress windows ke github](/upload-octopress-windows-ke-github/).
