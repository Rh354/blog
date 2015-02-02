---
layout: post
title: "Login SSH Tanpa Password"
date: 2015-01-20 10:25:53 +0700
comments: true
categories: tutorial
tags: [linux, tutorial]
fullview: false
---
Bagi anda yang pernah menggunakan linux berbasis teks, SSH sudah tentu akrab didengar. Tidak perlu menjelaskan panjang lebar tentang apa itu SSH, anda bisa membaca secara rinci pada link berikut [Wikipedia SSH](http://en.wikipedia.org/wiki/Secure_Shell). Sesuai judul pada tulisan ini, saya ingin menuliskan “bagaimana caranya mengkonfigurasi SSH agar saat diakses tidak perlu mengisi otentikasi”. Mungkin berguna bagi pembaca yang belum mengetahui, dan pastinya berguna bagi saya yang masih belajar. Instalasi SSH Server pada tiap distro linux yang hampir sama.

**Ubuntu**
{% highlight ruby %}
sudo apt-get install openssh-server
{% endhighlight %}

**Centos/RedHat**
{% highlight ruby %}
yum -y install openssh-server openssh-clients
{% endhighlight %}

**Arch**
{% highlight ruby %}
pacman -S openssh
{% endhighlight %}

Pada mayoritas distro linux, secara default SSH sebenarnya sudah terinstal. Yang perlu anda periksa adalah service SSH nya dalam keadaan Start, dan buka port ssh (secara default adalah port 22 namun bisa anda custom) pada firewall anda. Pada kondisi ini anda sudah bisa remote login ke SSH, namun masih harus mengisikan otentikasi. Jika anda masih dalam user root, saya menyarankan agar membuat user biasa. Kemudian login ke dalam user biasa tersebut. Selain pertimbangan keamanan, jika anda melakukan kesalahan hanya pada environment user biasa. Pada tulisan ini ada 2 buah linux yang ingin saya hubungkan melalui SSH tanpa harus mengisi password. Oleh karena itu saya membuat user yang sama yaitu “techshoot” pada kedua server.

{% highlight ruby %}
| Linux 1                               | Linux 2
| IP     : 192.168.10.20                | IP     : 192.168.10.21
| Hostname : cluster-00.techshoot.org   | Hostname : cluster-01.techshoot.org
{% endhighlight %}

Agar hostname bisa langsung ditranslasikan ke IP pada tiap linux. Anda harus mengedit berkas hosts yang ada pada /etc/hosts pada masing - masing linux.

{% highlight ruby %}
sudo su # masuk ke dalam mode root
echo 192.168.10.21 cluster-01.techshoot.org cluster-01 >> /etc/hosts # lakukan command ini pada Linux 1
echo 192.168.10.20 cluster-00.techshoot.org cluster-00 >> /etc/hosts # lakukan command ini pada Linux 2
{% endhighlight %}

Kemudian generate SSH keygen pada masing - masing linux.

{% highlight ruby %}
su -l techshoot # login sebagai user biasa, jika anda masih dalam user root
{% endhighlight %}

{% highlight ruby %}
ssh-keygen -t rsa # generate SSH Keygen
{% endhighlight %}

{% highlight ruby %}
Generating public/private dsa key pair.
Enter file in which to save the key (/home/techshoot/.ssh/id_rsa): # tekan enter
Enter passphrase (empty for no passphrase): # tekan enter
Enter same passphrase again: # tekan enter
Your identification has been saved in /home/techshoot/.ssh/id_rsa.
Your public key has been saved in /home/techshoot/.ssh/id_rsa.pub.
The key fingerprint is:
dd:15:ee:24:20:14:11:01:b8:72:a2:0f:99:4c:79:7f techshoot@localhost
The key's randomart image is:
+--[dsa 1024]---+
|     ..oB=.   .  |
|    .    . . . . |
|  .  .      . +  |
| oo.o    . . =   |
|o+.+.   S . . .  |
|=.   . E         |
| o    .          |
|  .              |
|                 |
+-----------------+
{% endhighlight %}

Merubah permissions file key pada masing - masing linux.

{% highlight ruby %}
sudo chmod 700 ~/.ssh/
{% endhighlight %}

{% highlight ruby %}
sudo chmod 600 ~/.ssh/id_rsa.pub
{% endhighlight %}

{% highlight ruby %}
sudo chmod 600 ~/.ssh/id_rsa
{% endhighlight %}

Kemudian masukkan baris - baris berikut :

{% gist Rh354/c9b5d498a1f5496a7e1b %}

Selanjutnya adalah copy public id linux ( remote server ). Karena saya ingin kedua linux saling berkomunikasi tanpa harus memasukkan password login ( “SSH passwordless” ). Saya menggunakan tools ssh-copy-id sebagai sarana memindahkan authorized_keys ke linux yang diakses.

**Menjalankan command ssh-copy-id pada Linux 1 ( cluster-00 )**

{% highlight ruby %}
ssh-copy-id -i ~/.ssh/id_rsa.pub techshoot@cluster-01.techshoot.org
{% endhighlight %}

{% highlight ruby %}
sudo chmod 600 ~/.ssh/authorized_keys
{% endhighlight %}

**Menjalankan command ssh-copy-id pada Linux 2 ( cluster-01 )**

{% highlight ruby %}
ssh-copy-id -i ~/.ssh/id_rsa.pub techshoot@cluster-00.techshoot.org
{% endhighlight %}

{% highlight ruby %}
sudo chmod 600 ~/.ssh/authorized_keys
{% endhighlight %}