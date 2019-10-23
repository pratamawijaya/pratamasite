---
layout: single
title:  "Mengatasi adb android no permission user in plugdev pada Linux"
date:   2019-10-23 09:00:00 +0700
excerpt: "adb android error no permission user in plugdev udev rules wrong"
categories: 
    - Linux
tags : 
    - android
    - programming
---

Pada artikel kali ini saya akan membahas bagaimana cara solving masalah adb no permission pada sistem operasi linux, contoh pada gambar dibawah ini, saat saya mencoba menjalankan perintah `adb devices` ternyata tidak berjalan dengan bagaimana semestinya.

![Adb No Permission](/assets/images/adb_error/adb_no_permission_1.png){:class="img-responsive"}

Berbekal hasil [googling](https://stackoverflow.com/questions/53887322/adb-devices-no-permissions-user-in-plugdev-group-are-your-udev-rules-wrong) akhirnya mendapatkan solusi, dari solusi itu saya coba tulis ulang dalam bahasa indonesia untuk keperluan dokumentasi dimasa mendatang.

Langkah pertama adalah mencari vendor id dari device android yang saya gunakan menggunakan perintah `lsusb`

![Mencari vendor id](/assets/images/adb_error/adb_2.png){:class="img-responsive"}

dari perintah tersebut vendor id = 2d95

selanjutnya adalah membuat udev rule, disini saya menggunakan text editor gedit

`$ sudo gedit /etc/udev/rules.d/51-android.rule`

lalu masukkan kode berikut

`SUBSYSTEM=="usb", ATTRS{idVendor}=="2d95", MODE="0666"`

dimana idVendor disesuaikan dengan id vendor device anda

![udev rule](/assets/images/adb_error/adb_4.png){:class="img-responsive"}

selanjutnya adalah reload rule

`$ sudo udevadm control --reload-rules`

lalu reconnect device anda dan check ulang menggunakan perintah `adb devices`

![device connected](/assets/images/adb_error/adb_3.png){:class="img-responsive"}

Sekian catatan singkat darisaya,

Terimakasih.
