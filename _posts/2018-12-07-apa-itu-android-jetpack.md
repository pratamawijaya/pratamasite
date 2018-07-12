---
layout: single
title:  "Tutorial Android : Mengenal Android Jetpack"
date:   2018-07-11 15:00:00 +0700
excerpt: "Apa itu android jetpack ? mari mengenal android jetpack melalui artikel ini"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
---

Pada pagelaran Google IO tahun 2018 ini, google kembali mengenalkan istilah baru di dunia **android programming**, kali ini mereka mengenalkan **Android Jetpack**

![Android jetpack](/assets/images/jetpack/android_jetpack_1.png){:class="img-responsive"}

Sebenarnya android jetpack itu apa ? pada situs halamannya [link](https://developer.android.com/jetpack/) android jetpack dijelaskan bahwasannya Android Jetpack merupakan kumpulan library, tools, dan panduan arsitektur untuk memudahkan dalam pengembangan aplikasi android.

![Android jetpack](/assets/images/jetpack/android_jetpack_2.png){:class="img-responsive"}

Secara garis besar android jetpack dibagi menjadi 4 kategori:

- Architecture
- UI
- Foundation
- Behavior

Bagi kalian yang sudah lama berkecimpung di dunia android development mungkin sudah tidak asing dengan data binding, lifecycles, appcompat, notifications, dll. Tahun ini google memutuskan untuk semakin merapikan architecture mereka dalam pengembangan android, dahulu kala untuk urusan architecture ini google sama sekali tidak ada panduan yang dapat digunakan, sehingga dari komunitas muncul banyak sekali gagasan gagasan mengenai architecture yang tepat ketika membuat aplikasi android, sebut saja android mvp architecture dari mindorks https://github.com/MindorksOpenSource/android-mvp-architecture 

Dengan munculnya **Android Jetpack** ini, diharapkan untuk rekan-rekan yang mulai belajar android programming dapat memanfaatkan panduan ini untuk membangun aplikasi android yang baik dan benar.

Untuk memulai membuat aplikasi android dengan Android Jetpack, dibutuhkan versi android studio 3.2 dimana versi ini sampai saat saya menulis artikel ini masih berupa preview, untuk membuatnya masih sama seperti android studio sebelumnya yang membedakan adalah saat memilih template, kita diberikan template baru dimana android studio akan membuatkan kita **Activity & Fragment + ViewModel**

![Android jetpack](/assets/images/jetpack/android_jetpack_3.png){:class="img-responsive"}

![Android jetpack](/assets/images/jetpack/android_jetpack_4.png){:class="img-responsive"}

selanjutnya android studio akan membuat 3 buah file class seperti ini

![Android jetpack](/assets/images/jetpack/android_jetpack_5.png){:class="img-responsive"}

saya akan coba jelaskan maksud dari class2 ini: 

- **MainActivity** : class utama dimana activity ini adalah activity yang pertama kali dipanggil oleh app, sebut saja activity ini adalah entry point aplikasi kalian
- **MainFragment** : class fragment yang mengurusi tampilan aplikasi kalian, misal mau menambah button ataupun text, ditambahkan dan dihandle melalui class ini
- **MainViewModel** : class untuk handling logic aplikasi kalian, jadi dengan menggunakan Android Jetpack ini kita **dipaksa** untuk menerapkan architecture MVVM (Model View View Model)

Lain kali akan saya coba buatkan artikel mengenai architecture mvvm ini, mohon ditunggu,
sekian artikel mengenai **Android Jetpack** kali ini, sekian dan terimakasih.

