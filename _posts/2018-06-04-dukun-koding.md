---
layout: single
title:  "Dukun Koding"
date:   2018-04-06 16:00:21 +0700
excerpt: "Sebuah cerita tentang sidejob seorang programmer yang menyambi menjadi dukun koding"
categories: 
    - Programming
tags : 
    - programming
    - android
---

Backup artikel beberapa tahun lalu

Hmm,, pada artikel kali ini agak gaje ya gaje, pada artikel ini akan sedikit mengandung sesi curhat, jadi siapkanlah tisu sebanyak – banyaknya, ceritanya gak akan kalah dengan drama korea hahaha :D sebelumnya mari kita baca ilustrasi dibawah ini

Mbah Joyo -> seorang dukun yang sudah berkecimpung dalam dunia java programming dan sekarang sedang mendalami java untuk android :D

Wiwid -> seorang mahasiswa akhir yang sedang berusaha mati matian menyelesaikan skripsinya a.k.a skripsi warrior

![Logcat](/assets/images/logcat.png){:class="img-responsive"}

Suatu ketika wiwid sedang berusaha menyelesaikan koding untuk skripsinya, dia sudah menyerah bingung dengan kodingannya sendiri, berusaha memperbaiki malah tambah error, matanya sudah memerah seperti errornya koding yang dia bikin, akhirnya dia mendepat selebaran tentang mbah joyo dukun koding yang sedang ngetop :D akhirnya wiwid memutuskan mendatangi mbah joyo :

Wiwid : tok tok.. mbah mbah..
Mbah Joyo : masuk cu.. kemari.. duduk disini.. simbah sudah tau kamu pnya masalah dengan kodingan kamu..
Wiwid : (Wah simbahnya udah tau) Koq udah tau mbah.. ?
Mbah Joyo : percuma cu, kalo saya dipanggil simbah tapi gak tau maksud kedatangan kamu kesini -_-”
Wiwid : eh iya juga mbah.. jadi gini mbah.. kodingan saya error, gak bisa running merah semua,,,, (sambil mau mengeluarkan laptop untuk ditunjukkan ke mbah joyo).
Mbah Joyo : stop stop.. gak usah dtunjukin kodenya..simbah sudah tau.. errornya karena apa.. itu karena di file MainActivity.java tidak kamu tambahkan titik koma pada baris ini dan coba kamu berikan komentar diatas method onCreate() mbah joyo ganteng.. ntar jalan
Wiwid :(buru2 nyatet dan coba dikodingannya) wah bener mbah.. langsung gak error makasih mbah :D

krik krik.. gak bener.. gak ada dukun koding seperti itu di dunia ini.. bisa memperbaiki kode tanpa tau penyebabnya.. nah kejadian seperti ini sering saya alami, ada beberapa pengunjung blog disini yang kirim email atau post komentar bilang, mas programku force close.. kenapa yah..? biasanya akan saya jawab dengan ‘duh gelap’ dan hasilnya dia kadang juga gak ngerti.

Oke tujuan awalnya untuk buat blog ini adalah untuk memberikan pengetahuan teman-teman tentang gimana jika ketemu error di java untuk android, dan bagaimana untuk memperbaikinya.

## 1. blablabla cannot be resolved or is blablabla

![Error](/assets/images/error_1.png){:class="img-responsive"}

bisa dilihat digambar diatas, error tersebut penyebabnya adalah compiler tidak menemukan resource menu dengan id search, seharusnya ada difolder res/menu/namafile.xml dengan identifier id search

## 2. Force Closed

![Error](/assets/images/error_force.png){:class="img-responsive"}

error seperti ini sering sekali terjadi, dan untuk pemula hal seperti ini bisa sangat membingungkan, mari telisik dari kodenya

{% gist 7956140 %}

untuk menangani error seperti ini bisa dilihat melalui logcat, contoh logcat seperti dibawah ini

![Error](/assets/images/view_logcat.png){:class="img-responsive"}

setiap logcat ada jenis2 levelnya, ada **D** untuk debug dan yang kita cari disini adalah yang memiliki **E** yaitu error

![Error](/assets/images/view_logcat_2.png){:class="img-responsive"}

terlihat disana terdapat nullpointer exception,

jika di scroll kebawah lagi, kita dapat menemukan dimana yang menyebabkan null pointer

![Error](/assets/images/view_logcat_3.png){:class="img-responsive"}

terlihat nullpointer disebabkan pada file MainActivity.java line 19
jika kita lihat didalam sourcenya,

![Error](/assets/images/src_1.png){:class="img-responsive"}

terlihat bawah btnSatu belum diinisiasi
rubah kode manjadi seperti dibawah ini :

![Error](/assets/images/src_2.png){:class="img-responsive"}

Jadi bagi kalian yang sudah baca ini, dan menemukan error, please jangan buat saya seperti dukun lagi, sertakan logcat erronya, jangan cuma bilang ‘mas aplikasiku gak jalan kenapa yah..?’ :D

Mohon maaf bila terdapat kesalahan
Sekian dan terima kasih


