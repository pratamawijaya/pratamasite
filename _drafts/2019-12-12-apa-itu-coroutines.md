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

Pada kesempatan kali ini saya akan membahas mengenai **Coroutines**, apa itu Coroutines ? Coroutines adalah pattern yang terdapat pada bahasa pemrograman kotlin untuk mengatasi masalah **concurrency**, sebuah cara baru untuk menulis secara asynchronous dan non-blocking code. Kotlin teams sendiri mendefinisikan coroutines adalah *lightweight threads*, 

Pada android coroutines dapat digunakan untuk mengatasi 2 problem utama :

..* Menghandle *heavy task* yang dapat menghalangi main thread sehingga mengakibatkan aplikasi lagging
..* Memberikan keamanan ketika memanggil *heavy task* dari *main thread*  contoh : networking, write ke database

<iframe src="https://pl.kotl.in/kzb9kadm-" width="500"></iframe>

Diatas adalah contoh kode kotlin yang menggunakan *coroutines*, jika kode tersebut dijalankan maka akan menghasilkan sebagai berikut :

```
Halooo
Dunia . . .
```
