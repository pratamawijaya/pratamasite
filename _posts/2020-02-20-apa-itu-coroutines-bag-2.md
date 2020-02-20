---
layout: single
title:  "Apa itu Coroutines ? Bagian 2"
date:   2020-02-20 12:00:00 +0700
excerpt: "Mengenal Coroutines bagian 2, membahas mengenai Coroutines Builder"
categories: 
    - android
tags : 
    - android
    - programming
    - kotlin
    - coroutine
---

# Menjalankan Coroutines

Untuk menjalankan coroutines kita membutuhkan **Coroutines Builder** beberapa coroutines builder

- runBlocking
- launch
- async

## runBlocking
Builder paling simple yang dapat digunakan namun tidak disarankan untuk digunakan di production code, builder ini akan memblocking thread yang sedang berjalan sampai fungsi coroutines yang ada didalamnya selesai dikerjakan

<iframe src="https://pl.kotl.in/EE-UcoJSJ"></iframe>

pada contoh diatas ketika program dijalankan akan langsung memuncullkan text "Halo" lalu akan ada jeda selama 3 detik kemudian baru memunculkan text "Dunia"

## launch
Builder satu ini adalah builder yang cocok untuk mengerjakan suatu task secara async, atau lebih dikenal dengan istilah "Fire n Forget", contoh penggunaan builder launch

<iframe src="https://pl.kotl.in/uG7hvhCQm"></iframe>

program pertama kali akan menjalankan coroutines yang ada didalam builder launch, karena saya beri delay selama 1 sec, maka akan ada jeda waktu, ketika jeda tersebut program tetap menjalankan perintah selanjutnya yaitu print Helo, maka program akan menampilkan text Helo, kemudian ketika jeda 1 sec telah terlampui maka tampilan text Dunia akan muncul, runblocking pada akhir program digunakan untuk memastikan jvm masih tetap hidup sehingga muncul semua textnya, jika tidak dilakukan delay maka yang tampil hanya text Halo

## async
Builder ini digunakan ketika kita membutuhkan kembalian nilai dari sebuah suspend function, contohnya seperti ini

<iframe src="https://pl.kotl.in/ezn6cJ8te"></iframe>

pada baris 11, perintah print akan menunggu result tersedia dulu yang mana result adalah hasil dari builder function di baris 5, caranya adalah dengan menambahkan ".await()"