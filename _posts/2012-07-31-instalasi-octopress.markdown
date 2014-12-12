---
layout: post
title: "instalasi octopress"
date: 2012-07-31 04:48
comments: true
fullview: false
categories: tutorial
tags: [tutorial, linux]
---

Berawal dari tulisan seorang suhu yang ada di blog [http://blog.mahirrudin.com/](http://blog.mahirrudin.com/) akhirnya nubi ini memberanikan untuk mencoba menginstall octopress ini menggantikan wordpress. baik kita mulai saja PDKT dengan octopressnya.

>**Octopress** adalah framework yang dibuat oleh Brandon Mathis untuk Jekyll, dirancang untuk website statis pada Github Pages. Untuk dapat “Blogging” ( menulis pada Jekyll ) diperlukan pemahaman akan CSS3, HTML5 serta Javascript. Namun, dengan octopress anda tidak perlu pemahaman khusus karena semua bisa di generate secara otomatis. 

begitulah singkatnya kira - kira. namun karena memakai sistem ruby maka kita perlu menginstall beberapa dependencies terkait dengan kelancaran menginstal octopress. Berikut instalasinya.

**Instal Dependenciesnya :**

**Untuk ubuntu**
{% highlight ruby %}
$ sudo apt-get install bash curl git-core build-essential bison openssl libreadline6 libreadline6-dev zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake
{% endhighlight %}

**Untuk CentOS/RedHat**
{% highlight ruby %}
$ sudo yum groupinstall "Development Tools" && sudo yum install bison curl bash readline-devel zlib zlib-devel openssl openssl-devel libyaml git python-setuptools python-devel
{% endhighlight %}

**Install Rbenv dan Plugin Ruby Build**

**Install Rbenv**
{% highlight ruby %}
$ cd $HOME
$ git clone git://github.com/sstephenson/rbenv.git $HOME/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> $HOME/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> $HOME/.bash_profile
$ source $HOME/.bash_profile
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> $HOME/.bashrc
$ echo 'eval "$(rbenv init -)"' >> $HOME/.bashrc
$ source $HOME/.bashrc
$ mkdir -p $HOME/.rbenv/plugins
$ cd $HOME/.rbenv/plugins
$ git clone git://github.com/sstephenson/ruby-build.git
$ cd $HOME/.rbenv/plugins/ruby-build
$ sudo sh install.sh
$ exec $SHELL
{% endhighlight %}

**Install Ruby 1.9.2**

{% highlight ruby %}
$ rbenv install 1.9.2-p290
{% endhighlight %}

**Setup Octopress**

{% highlight ruby %}
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
$ gem install bundler
$ rbenv rehash
$ bundle install
$ rake install
{% endhighlight %}

Octopress telah berhasil diinstall namun belum selesai di setup, anda harus konfigurasi file `_config.yml` dan lebih jelasnya anda bisa membaca di [Configuring Octopress](http://octopress.org/docs/configuring/). Berikut contoh sederhana confignya.

{% highlight ruby %}
url: http://techshoot.org/
title: techshoot Blog
subtitle: cuma coretan iseng
author: techshoot
simple_search: http://google.com/search
description: Tips and Trik, tutorial
{% endhighlight %}

nah jika sudah selesai hasil generatenya nanti yang berupa **.html** di folder **public** dan bisa langsung diupload ke hostingan masing - masing.
