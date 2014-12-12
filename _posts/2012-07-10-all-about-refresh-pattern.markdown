---
layout: post
title: "All About Refresh Pattern"
date: 2012-07-10 16:44
comments: true
categories: knowledge
tags: [linux, windows, tutorial, squid]
fullview: false
---

Mekanisme umum akses internet via web browser utk menguji validitas **"freshness"** obyek yg tersimpan adl dg 'menjenguk' obyek tsb ke server asal/sumber dan membandingkannya dg obyek yg sama yg sudah tersimpan di lokal. Jadi memang 'boros' walaupun belum tentu setiap saat obyek tersebut diambil lho, jadi hanya sekedar ditanyai saja 'tgl lahirnya' (dan ada beberapa info yg lain yg penting juga, tapi kita fokus di 'umur'nya saja). Dg demikian saran utk teman-teman yg mengelola warnet adl dg memperbesar cache lokal web browser (internet temporary files-nya utk IE dan tidak salah default-nya 10% dari ukuran partisinya ya?). 

Kenapa kok 'umur' obyek (atau halaman web) diuji? Ya kira2 jawabannya supaya informasinya selalu yg terbaru dan tidak salah (kalau tidak mau yg terbaru dan selalu hanya mengakses yg sudah tersimpan di lokal cache web browser ya di-set saja mode "offline", dijamin akses akan sangat lebih cepat dan sangat hemat bandwidth, tapi dg konsekuensi ekstrim spt halaman tsb di server sudah dihapus, di sisi klien bakalan tidak tahu lho). 

Kepingin mjd "super boros' dg asumsi tanpa Squid atau cache server (bukan lokal cache lho)? ya cache di web browser dimatikan saja shg setiap saat akses internet akan selalu mengambil obyek/halaman langsung dari server asal/sumbernya (tapi irit tempat di hardisk lokal, hanya ini keuntungannya). Jelas dari sisi latensi akan naik drastis. Kira2 sudah bisa dibayangkan ya? hi hi hi (makanya beberapa ISP secara diam2 akan men"transparan"kan cache server dg maksud mau membantu ekstrimis yg "super boros" ini tadi, he he he apa hanya sekedar alasan buat mereka menghemat bw ya?)

Baik skrg masuk peran Squid yg pada dasarnya bersifat "shared" obyek lokal dalam konteks 1 domain. Secara garis besar mekanisme kerjanya mirip dg "temporary cache" lokal web browser itu tadi cuma bedanya dipakai bersama dg user yg lain. Yg membedakan dalam kaitannya dg refresh_pattern adl Squid tidak tidak akan "bertanya" validitas obyek jika ternyata "umur"nya masih dalam durasi refresh_pattern-nya (default minimum Squid 120 menit atau 2 jam, kalau tidak salah). 

Jadi jika "umur" obyek sejak di"lahir"kan belum mencapai 2 jam (utk contoh) maka Squid tidak akan mengakses server asal/sumber utk mengambil info "umur"nya, dg kata lain obyek akan dianggap masih valid selama durasi waktu tadi (2 jam, misalnya), alias penghematan bw dan peningkatan responsivitas akses.

Kerugian jika refresh_pattern minimum (min) terlalu lama, misalnya kita set 1 hari, jelas jika ternyata dalam waktu kurang dari 1 hari obyek di ujung server asal berubah, di sisi klien dan Squid-nya masih akan tetap dianggap valid, atau dg kata lain, informasinya salah/tidak akurat, lha halaman web-nya memang tidak sama dg yg di server. Bagi web desainer, contoh saja lho tanpa ada masuk diskriminasi, he he he akan tidak suka krn ada kebutuhan "instan" setiap kali mengubah atau memperbaiki halaman web di ujung server utk keperluan evaluasi (ya jelas tidak akan mau menunggu 1 hari utk melihat perubahannya, ha ha ha benar ya?). 

Jadi intinya min refresh_pattern adalah keterangan kapan waktu "tersegera" utk menguji validitas obyek. Jika obyek teruji masih valid, Squid akan mengambil dari lokal cache swap-nya, jika obyek sudah tidak valid ya jelas Squid akan mengambil obyek dari server asal. Sekarang masalahnya, bagaimana jika obyek tidak memiliki "umur" atau info "tgl lahir" (tidak semua web itu memiliki info ini lho, tergantung si web programernya).

Kapan menguji atau mengambil langsung dari server asal obyek "tak berumur" ini, toh validitasnya tidak bisa diuji? Persentase dan nilai maksimum-lah yg akan menentukan (percentage max). Obyek tanpa umur ini tadi akan dianggap valid oleh Squid selama umur minimumnya 50% dari umur maksimumnya, misalnya. Utk contoh 50% 120 akan berarti obyek tanpa umur valid selama"umur"nya masih kurang dari 1 jam (50% dari 120 menit). 

Menurut pengalaman perubahan obyek2 HTTP di internet itu relatif ";lamban", maka kebiasaan gw pribadi demi Squid yg "agresif" adalah antara 80% s/d 95% dg nilai maksimum hingga 1 bulan (berapa menit ya, sori lupa) dan utk FTP krn semakin jarang berubah bisa lebih lama lagi bisa hingga 3 bulan atau 6 bulan. Tentunya ini tergantung profil pengguna internet anda lho, hanya contoh ekstrim saja. Jadi saat "umum" maksimum yg sudah didefinisikan di refresh_pattern tercapai, jelas Squid akan"menjenguk" obyek tsb ke asal servernya. Dg asumsi obyek masih sama maka Squid akan mengambil dari lokal cache swap-nya. Jika ternyata obyek sudah berbeda, misalnya dari ukuran file atau saat file obyek tsb berbeda, maka Squid akan mengambil dari server tsb.

Opsi override-lastmod dan reload-into-ims kepanjangannya adalah "**override last modification" dan reload into if-modified-since"**. Override-lastmod akan meng"override"; perubahan yg terjadi di server asal obyek dg mengabaikan validitasnya hingga minimum refresh patternnya tercapai. Efeknya obyek di lokal Squid bisa berbeda dg obyek yg di server asal. Tapi opsi ini masih mengijinkan si user "memaksa" menguji validitasnya dg menekan tomboh **reload** atau **refresh** di web browser. Opsi ini, kalau gw memandangnya ***"agak menipu sedikit***, he he he 

Sbg contoh di atas, gw yakin obyek di ujung server sudah berubah walaupun terakhir gw akses baru 10 menit yg lalu (min refresh_pattern=120 menit, misalnya), maka dg menekan tombol **reload** di browser gw akan bisa menguji validitas obyek tsb dg yg di server asal, dan jika ternyata ya benar obyek tsb sudah berubah, jelas Squid akan langsung mengambil obyek lebih baru dari server asal. Jika gw biarkan saja akses ke obyek tsb tanpa menekan tombol **reload** di browser maka mekanisme uji validitas mempergunakan min refresh_pattern akan berlaku biasa.

Opsi reload-into-ims akan mengubah atau memodifikasi "umur" obyek sehingga seakan-akan **dilahirkan** kembali atau **direset**. Misalnya gw pernah mengakses suatu obyek suatu obyek 1,5 jam yg lalu, dan gw akses lagi obyek yg sama sekarang, maka **umur** obyek yg sama ini akan dianggap**fresh** atau obyek baru dan sudah tidak berumur 1,5 jam yg lalu. Keuntungan opsi ini adalah mekanisme **penyegaran** umur obyek populer sehingga tidak pernah mjd **tua** hingga nilai percentage dan maksimumnya tercapai, sekali lagi**agak menipu**, he he he.. Kendali penuh tetap di user utk menekan tombol **reload** atau **refresh** jika tidak yakin obyek **fresh**.</p>

**Simpelnya :**

Refresh_pattern itu sebenarnya buat ngukur seberapa lama sih object itu tersimpan di cache squid kita, krg lebih artinya umur objectnya gitu deh, krn pada dasarnya beberapa object kadang selalu terupdate di server websitenya.

Umumnya penulisan refresh_pattern tercantum min percent max (sprti: 720 100% 4320), tentunya ada opsi2 lain dibelakangnya. min dan max itu nilainya menit. Min itu batas minimum object harus direfresh (diupdate dr server), sedangkan max itu batas maximal object harus direfresh, kalo persennya itu utk perhitungan jika umur object berada di antara min dan max.

Jadi bgini, sewaktu kita akan merequest atau membuka kembali sbuah object yg sudah tersimpan di cache, maka squid akan menilai umur object berdasarkan refresh patternnya.

kalo umur object masih berada di bawah nilai min, maka object itu dianggap masih bs dipake kembali tanpa perlu ngecek ke server. Artinya, "ah.. ni object masih seger lah baru td disimpen kok, gak usah ngecek ke server websitenya deh, ambil aja langsung trus tempel di halaman web"

Tp klo umur object berada di atas nilai max, maka squid akan melakukan pengecekan ke server dan meminta object agar diupdate (refresh).

Nah, bagaimana kalo object yg sedang diminta itu umurnya berada di antara min dan max. Di sinilah nilai persentase itu dipake, persentase itu maksudnya adalah persentase umur object di cache dibandingkan dengan umur object di servernya. tp gimana njelasinnya ya, pke contoh aja yak:

>ada sebuah objek, refresh_pattern nya 20 75% 300. 

>object ini dibikin di servernya jam 1:00, 

>object pertama kali direquest dan tersimpan dalam cache jam 1:30

>object direquest kembali jam 1:45

Nah brarti umur object di cachenya adlh 15 menit (1:45-1:30)

squid blm bs menentukan object ini kadaluarsa apa blm krn berada di antara batas min (10) dan max (300). Nah kali ini pake persentase, jd perhitungannya ya hrs pake persen jg (bkn menit).

mari kita hitung persentase umur object:

>umur object di cache: 15 menit (1:45-1:30)

>umur object di server: 45 menit (1:45-1:30)

>brarti persennya: 15/45 = 33%

Kesimpulannya, object dianggap masih fresh krn masih berada di bawah 75%.