---
layout: post
title: "Compile FFMPEG x265 HEVC"
date: 2015-02-12 10:25:53 +0700
comments: true
categories: tutorial
tags: [linux, tutorial]
fullview: false
---
Tutorial ini dibuat untuk keperluan pribadi dengan [serviio](http://serviio.org/) sebagai DLNA Server di ubuntu 12.04 dan kebetulan ffmpeg yang terinstall tidak support x265 atau HEVC. Oleh karena itu dengan modal nekat dan google akhirnya bisa juga compile nya di ffmpeg terbaru. 

Pada saat tutorial ini dibuat linux yang dipakai adalah `Ubuntu 12.04` dan `Serviio 1.5`, untuk build **x265/HEVC** dibutuhkan beberapa hal yang harus terpenuhi yaitu **cmake** minimal 2.8.8 dan **yasm** minimal 1.2, di ubuntu 12.04 versi yang ada di repository tidak memenuhi kriteria oleh karena itu kita harus melakukan compile versi tersebut.

Pertama - tama uninstall terlebih dahulu ffmpeg yang lama.
  
{% highlight ruby %}
sudo apt-get remove ffmpeg x264 libx264-dev libvpx-dev librtmp0 librtmp-dev
{% endhighlight %}

**NOTE :**

Jika pada saat melakukan remove jumlahnya berkisar **500 Mb** silakan di **cancel** dan hilangkan **librtmp0** dari command diatas dan jalankan kembali command diatas.

Install tools yang diperlukan.

{% highlight ruby %}
sudo apt-get update
{% endhighlight %}

{% highlight ruby %}
sudo apt-get install build-essential checkinstall git libfaac-dev libjack-jackd2-dev \
  libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libsdl1.2-dev libtheora-dev \
  libva-dev libvdpau-dev libvorbis-dev libx11-dev libxfixes-dev texi2html zlib1g-dev \
  libssl1.0.0 libssl-dev libxvidcore-dev libxvidcore4 libass-dev cmake mercurial
{% endhighlight %}

**Build dan Install YASM**

Rekomendasi yasm yang digunakan adalah 1.2 ke atas, di tutorial ini yang digunakan adalah yang terakhir yaitu yasm 1.3.

{% highlight ruby %}
cd ~
{% endhighlight %}

{% highlight ruby %}
mkdir src
{% endhighlight %}

{% highlight ruby %}
cd src
{% endhighlight %}

{% highlight ruby %}
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
{% endhighlight %}

{% highlight ruby %}
tar xzvf yasm-1.3.0.tar.gz
{% endhighlight %}

{% highlight ruby %}
cd yasm-1.3.0
{% endhighlight %}

{% highlight ruby %}
./configure
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=yasm --pkgversion="1.3.0" --backup=no \
  --deldoc=yes --fstrans=no --default
{% endhighlight %}

**Build dan Install CMAKE**

Rekomendasi cmake yang digunakan adalah minimal 2.8.8 ke atas, di tutorial ini yang digunakan adalah yang terakhir yaitu cmake 3.1.2.

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
wget http://www.cmake.org/files/v3.1/cmake-3.1.2.tar.gz
{% endhighlight %}

{% highlight ruby %}
tar xzvf cmake-3.1.2.tar.gz
{% endhighlight %}

{% highlight ruby %}
cd cmake-3.1.2
{% endhighlight %}

{% highlight ruby %}
mkdir _build
{% endhighlight %}

{% highlight ruby %}
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo make install  
{% endhighlight %}

{% highlight ruby %}
sudo ldconfig
{% endhighlight %}

cek versinya dengan command berikut

{% highlight ruby %}
cmake --version
{% endhighlight %}

**Build dan Install x264**

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
git clone git://git.videolan.org/x264
{% endhighlight %}

{% highlight ruby %}
cd x264
{% endhighlight %}

{% highlight ruby %}
./configure --enable-static --disable-opencl
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=x264 --pkgversion="3:$(./version.sh | \
    awk -F'[" ]' '/POINT/{print $4"+git"$5}')" --backup=no --deldoc=yes \
    --fstrans=no --default
{% endhighlight %}

check apabila telah terinstall x264

{% highlight ruby %}
x264 --help
{% endhighlight %}

kalau sudah keluar helpnya maka terinstall dan tidak ada masalah

**Build dan Install librtmp**

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
git clone git://git.ffmpeg.org/rtmpdump
{% endhighlight %}

{% highlight ruby %}
cd rtmpdump
{% endhighlight %}

{% highlight ruby %}
make SYS=posix
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=rtmpdump --pkgversion="2:$(date +%Y%m%d%H%M)-git" --backup=no \
    --deldoc=yes --fstrans=no --default
{% endhighlight %}

**Build dan Install libvpx**

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
git clone http://git.chromium.org/webm/libvpx.git
{% endhighlight %}

{% highlight ruby %}
cd libvpx
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=libvpx --pkgversion="1:$(date +%Y%m%d%H%M)-git" --backup=no \
    --deldoc=yes --fstrans=no --default
{% endhighlight %}

**Build dan Install x265/HEVC**

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
hg clone http://hg.videolan.org/x265
{% endhighlight %}

{% highlight ruby %}
cd x265/build/linux
{% endhighlight %}

{% highlight ruby %}
./make-Makefiles.bash
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo make install
{% endhighlight %}

check apabila telah terinstall x265

{% highlight ruby %}
x265 --help
{% endhighlight %}

kalau sudah keluar helpnya maka terinstall dan tidak ada masalah, namun jika ada notif error seperti berikut

{% highlight ruby %}
x265: error while loading shared libraries: libx265.so.45: cannot open shared object file: No such file or directory
{% endhighlight %}

maka solusinya adalah

{% highlight ruby %}
cd /lib
{% endhighlight %}

{% highlight ruby %}
sudo ln -s /usr/local/lib/libx265.so.45
{% endhighlight %}

{% highlight ruby %}
cd -
{% endhighlight %}

untuk mengecek parameter apa yang di lihat tergantung errornya, contoh error diatas berhubungan dengan file **libx265.so.45**, silakan sesuaikan nama filenya jika berbeda.

Langkah selanjutnya adalah build **FFMPEG**

{% highlight ruby %}
cd ~/src
{% endhighlight %}

{% highlight ruby %}
git clone --depth 1 git://git.videolan.org/ffmpeg
{% endhighlight %}

{% highlight ruby %}
cd ffmpeg
{% endhighlight %}

{% highlight ruby %}
./configure --enable-gpl --enable-libfaac --enable-libmp3lame --enable-libopencore-amrnb \
    --enable-libopencore-amrwb --enable-libtheora --enable-libvorbis --enable-libx264 \
    --enable-nonfree --enable-postproc --enable-version3 --enable-x11grab --enable-librtmp \
    --enable-libxvid --enable-libass --enable-libx265 --enable-libvpx
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=ffmpeg --pkgversion="99:$(date +%Y%m%d%H%M)-git" --backup=no \
  --deldoc=yes --fstrans=no --default
{% endhighlight %}

Ketika FFMPEG telah terinstall selanjutnya kita build dan install  **x264 LAVF support**

{% highlight ruby %}
sudo apt-get remove x264
{% endhighlight %}

{% highlight ruby %}
cd ~/src/x264
{% endhighlight %}

{% highlight ruby %}
rm *.deb
{% endhighlight %}

{% highlight ruby %}
make distclean
{% endhighlight %}

{% highlight ruby %}
./configure --enable-static --disable-opencl
{% endhighlight %}

{% highlight ruby %}
make
{% endhighlight %}

{% highlight ruby %}
sudo checkinstall --pkgname=x264 --pkgversion="3:$(./version.sh | \
    awk -F'[" ]' '/POINT/{print $4"+git"$5}')" --backup=no --deldoc=yes \
    --fstrans=no --default
{% endhighlight %}

reboot Ubuntu anda

jika nanti muncul error **ffmpeg: error while loading shared libraries: librtmp.so.1: cannot open shared object file: No such file or directory**, lakukan langkah berikut

{% highlight ruby %}
sudo nano /etc/ld.so.conf 
{% endhighlight %}

add baris berikut

{% highlight ruby %}
include /usr/local/lib/
{% endhighlight %}

save file dan ketik command berikut

{% highlight ruby %}
sudo ldconfig 
{% endhighlight %}

reboot ubuntu anda

Untuk Mengetahui apakah FFMPEG anda telah support x265/HEVC jalankan command berikut

{% highlight ruby %}
ffmpeg -codecs | grep -i hevc
{% endhighlight %}

maka hasilnya 

{% highlight ruby %}
libavutil      	54. 18.100 / 54. 18.100
libavcodec   	56. 21.102 / 56. 21.102
libavformat  	56. 19.101 / 56. 19.101
libavdevice  	56.  4.100 / 56.  4.100
libavfilter    	 5. 10.100 /  5. 10.100
libswscale     	 3.  1.101 /  3.  1.101
libswresample  	 1.  1.100 /  1.  1.100
libpostproc  	53.  3.100 / 53.  3.100
DEV.L. hevc   	H.265 / HEVC (High Efficiency Video Coding) (encoders: libx265 )
{% endhighlight %}

selamat x265/HEVC sudah terinstall di ubuntu anda

Berikut maksud dari kode diatas

{% highlight ruby %}
Codecs:
D..... = Decoding supported
.E.... = Encoding supported
..V... = Video codec
..A... = Audio codec
..S... = Subtitle codec
...I.. = Intra frame-only codec
....L. = Lossy compression
.....S = Lossless compression
{% endhighlight %}

**Link Referensi :**

[http://wiki.serviio.org/doku.php?id=build_ffmpeg_linux](http://wiki.serviio.org/doku.php?id=build_ffmpeg_linux)
[http://www.rover12421.com/](http://www.rover12421.com/)