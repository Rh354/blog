---
layout: post
title: "Install Jekyll"
date: 2014-11-10 15:03:53 +0700
comments: true
categories: tutorial
tags: [linux, tutorial]
fullview: false
---
Apa itu jekyll, menurut sitenya jekyll adalah

>Jekyll is a simple, blog-aware, static site generator. It takes a template directory containing raw text files in various formats, runs it through Markdown (or Textile) and Liquid converters, and spits out a complete, ready-to-publish static website suitable for serving with your favorite web server. 

Pada artikel kali ini saya akan share cara install **jekyll** di linux. untuk distro yang di pakai kali ini adalah ubuntu.


untuk requirementnya diperlukan hal berikut

- [Ruby](http://www.ruby-lang.org/en/downloads/)
- [RubyGems](http://rubygems.org/pages/download)
- Linux, Unix, MAC OS X
- [NodeJS](http://nodejs.org/)

berikut langkahnya

- install terlebih dahulu requirementnya

{% highlight ruby %}
sudo apt-get install ruby ruby-dev make
{% endhighlight %}

- install jekyll

{% highlight ruby %}
gem install jekyll
{% endhighlight %}

secara keseluruhan untuk langkah install sampai disini cukup. jika ingin build website maka jalankan langkah berikut di terminal

{% highlight ruby %}
jekyll new my-awesome-site
{% endhighlight %}

ganti tulisan **my-awesome-site** menjadi nama calon blog anda misal **techshoot.org**

untuk membuat post silakan baca - baca dilink [documentationnya](http://jekyllrb.com/docs/usage/)

untuk melihat previewnya langsung execute command berikut

{% highlight ruby %}
jekyll serve -H 0.0.0.0
{% endhighlight %}

jika sudah yakin maka bisa langsung di build dengan command berikut

{% highlight ruby %}
jekyll build
{% endhighlight %}

untuk template lainnya silakan cek di site [Jekyll Themes](http://jekyllthemes.org/)
