---
layout: post
title: "upload octopress windows ke github"
date: 2014-01-27 10:48:04 +0700
comments: true
categories: tutorial
tags: [windows, octopress, tutorial]
fullview: false 
--- 

###Buat Account GITHUB###
Buat account terlebih dahulu di github melalui link berikut **[ini](https://github.com/plans)** dan klik **sign up**.

###Generate dan import SSH key ke Github###
Pada github, untuk mengupload konten semuanya dilakukan melalui **SSH**, untuk itu diperlukan *public key* dari pengakses (komputer anda) sebagai autentikasi. untuk panduan SSH di github tersebut anda dapat langsung melihatnya di **[Github Generating ssh keys](https://help.github.com/articles/generating-ssh-keys)**.

###Membuat repository###
Untuk mengupload file - file Octopress ke Github, anda memerlukan directory dengan akses read and write. Oleh karena itu anda harus membuat Repository, cara membuat repository bisa anda baca disini [Create New Repository](https://help.github.com/articles/creating-a-new-repository). dan dalam contoh berikut username yang dibuat adalah **techshoot**

###Deploy Octopress###
silakan masuk ke direktori octopress anda via command prompt sesuai dengan artikel sebelumnya dan masukkan command yang di generate pada page Github saat anda membuat Repository

{% highlight ruby %}
cd techshoot.github.com
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin https://github.com/your-username/your-username.github.com.git
{% endhighlight %}

Saat membuat **"Repository"** `your-username.github.com`, anda akan diberikan beberapa pilihan. Anda cukup memilih **Next steps**:, tapi ada beberapa command yang harus disesuaikan dengan konfigurasi pada octopress anda. Berikut adalah command - command yang saya pakai, dan sudah sesuai dengan tutorial ini :

{% highlight ruby %}
C:\github>cd techshoot.github.com #techshoot.github.com merupakan direktori octopress yang dibuat artikel ini
C:\github\techshoot.github.com>git init
C:\github\techshoot.github.com>touch README
C:\github\techshoot.github.com>git add README
C:\github\techshoot.github.com>git commit -m 'first commit'
C:\github\techshoot.github.com>git remote add techshoot.github.com https://github.com/techshoot/techshoot.github.com.git
{% endhighlight %}

**NB : techshoot.github.com adalah folder octopress anda**

Setelah itu anda bisa klik **"continue"** pada halaman Gihub repository setup. Langkah selanjutnya adalah setup_github_pages di folder octopress anda :

{% highlight ruby %}
C:\github\techshoot.github.com>rake setup_github_pages
{% endhighlight %}

{% highlight ruby %}
Enter the read/write url for your repository: git@github.com:techshoot/techshoot.github.com.git
Added remote git@github.com:techshoot/techshoot.github.com.git as origin
Set origin as default remote
Master branch renamed to 'source' for committing your blog source files
Initialized empty Git repository in C:/github/techshoot.github.com/_deploy/.git/
[master (root-commit) 2a4e9e7] Octopress init
1 files changed, 1 insertions(+), 0 deletions(-)
create mode 100644 index.html
{% endhighlight %}

langkah selanjutnya bisa langsung di `rake deploy`, melalui command prompt atau windows powershell ketik

{% highlight ruby %}
C:\github\techshoot.github.com>rake generate
C:\github\techshoot.github.com>rake deploy
{% endhighlight %}

Kembali ke directory octopress anda, lakukan hal berikut setiap anda selesai menulis postingan atau merubah konfigurasi:

{% highlight ruby %}
rake generate
rake deploy
{% endhighlight %}

jika rake deploy ada error dengan tulisan `git config --global user.email "your.example@mail.com"`, maka jalankan

{% highlight ruby %}
git config --global user.email "techshoot@nubi.com" #silakan ganti techshoot@nubi.com menjadi mail yang anda daftarkan di github anda pada saat pembuatan account
{% endhighlight %}

selanjutnya jika ingin merubah domain github, silakan simak artikel berikut [Ubah Domain Github Menjadi Domain Sendiri]({{site.baseurl}}{% post_url 2014-01-27-ubah-domain-github-menjadi-domain-sendiri %})