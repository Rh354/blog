---
layout: post
title: "ubah domain github menjadi domain sendiri"
date: 2014-01-27 13:24:18 +0700
comments: true
fullview: false 
categories: tutorial
tags: [windows, octopress, tutorial]
---
Github support custom domain. Jika sebelumnya Octopress anda beralamat http://username.github.com, maka bisa diubah menjadi http://domainanda.com. Berikut caranya :

- masuk ke folder source octopress anda, di dalam artikel ini folder tersebut berada di `C:\github\octopress`
- Buat sebuah file dengan nama `CNAME` melalui notepad tanpa memakai extension apapun.
- isi file tersebut dengan nama domain anda. misal *techshoot.org* atau *www.techshoot.org* (di dalam contoh memakai domain techshoot.org, silakan di kreasikan sendiri).

Rubah DNS registar domain anda, untuk pointing domain ke IP github. Selengkapnya bisa anda baca disini [setting-up-a-custom-domain-with-pages](https://help.github.com/articles/setting-up-a-custom-domain-with-pages).