---
layout: single
title:  "Coroutines : Apa itu coroutines ?"
date:   2019-12-12 09:00:00 +0700
excerpt: "Mengenal coroutines pada pemrograman dengan bahasa kotlin"
categories: 
    - Linux
tags : 
    - android
    - programming
---

Pada kesempatan kali ini saya akan membahas mengenai **Coroutines**, apa itu Coroutines ? Coroutines adalah pattern yang terdapat pada bahasa pemrograman kotlin untuk mengatasi masalah **concurrency**. Pada android coroutines dapat digunakan untuk mengatasi 2 problem utama :

..* Menghandle *heavy task* yang dapat menghalangi main thread sehingga mengakibatkan aplikasi lagging
..* Memberikan keamanan ketika memanggil *heavy task* dari *main thread*  contoh : networking, write ke database

<iframe src="https://pl.kotl.in/oWd7XfUy4" width="500"></iframe>