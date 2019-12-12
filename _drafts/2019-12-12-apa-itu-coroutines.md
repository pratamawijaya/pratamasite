---
layout: single
title:  "Coroutines : Apa itu coroutines ?"
date:   2019-12-12 09:00:00 +0700
excerpt: "Mengenal coroutines pada pemrograman dengan bahasa kotlin"
categories: 
    - android
tags : 
    - android
    - programming
---

Pada kesempatan kali ini saya akan membahas mengenai **coroutines**, apa itu Coroutines ? Coroutines adalah pattern atau salah satu cara untuk mengatasi masalah *concurrency* atau *async* programming, Kotlin teams sendiri menyebut coroutines adalah *lightweight thread*, jadi coroutines ini mirip *thread* tapi ada perbedaannya, persamaan antara coroutines dan thread adalah :

- sama-sama *sequence of instruction*
- multiple coroutines atau thread dapat dijalankan secara concurent
- multiple coroutines atau thread sharing resource memory
  
dan untuk perbedaannya adalah :
- coroutines disebut *lightweight thread*
- coroutines jalan diatas thread

Penjelasan mengenai lightweight thread, thread biasanya dibandingkan dengan lamanya waktu yang digunakan untuk pemrosesan data, jadi *lightweight thread* adalah thread yang membutuhkan waktu lebih singkat dalam pemrosesannya sedangkan *heavyweight thread* adalah kebalikannya butuh waktu lebih banyak untuk pemrosesannya. Dan selain itu thread akan selalu dihandle oleh operating system sedangkan coroutines dihandle oleh user.

Coroutines tidak hanya ada pada bahasa pemrograman kotlin, namun juga ada dibahasa lain, ada 2 jenis coroutines yaitu :

- Stackless
- Stackfull
  
Kotlin menggunakan coroutines yang berjenis *stackless*.

Pada android coroutines dapat digunakan untuk mengatasi 2 problem utama :

..* Menghandle *heavy task* yang dapat menghalangi main thread sehingga mengakibatkan aplikasi lagging
..* Memberikan keamanan ketika memanggil *heavy task* dari *main thread*  contoh : networking, write ke database

<iframe src="https://pl.kotl.in/kzb9kadm-" width="500"></iframe>

Diatas adalah contoh kode kotlin yang menggunakan *coroutines*, jika kode tersebut dijalankan maka akan menghasilkan sebagai berikut :

```
Halooo
Dunia . . .
```
