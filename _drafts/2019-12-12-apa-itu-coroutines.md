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
    - kotlin
    - coroutine
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
  
Kotlin menggunakan coroutines yang berjenis *stackless*. [link terkait stackless & stackfull](https://blog.varunramesh.net/posts/stackless-vs-stackful-coroutines/) [link](https://stackoverflow.com/questions/28977302/how-do-stackless-coroutines-differ-from-stackful-coroutines)

Pada android coroutines dapat digunakan untuk mengatasi 2 problem utama :

- Menghandle *heavy task* yang dapat menghalangi main thread sehingga mengakibatkan aplikasi lagging
- Memberikan keamanan ketika memanggil *heavy task* dari *main thread*  contoh : networking, write ke database

# Contoh Coroutine

<iframe src="https://pl.kotl.in/kzb9kadm-" width="500"></iframe>

Diatas adalah contoh kode kotlin yang menggunakan *coroutines*, jika kode tersebut dijalankan maka akan menghasilkan sebagai berikut :

```
Halooo
Dunia . . .
```

Pada contoh diatas, pada kode `1` saya menggunakan `GlobalScope.launch` apakah ini ? untuk menjalankan coroutine dibutuhkan [Coroutine Scope](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/coroutine-scope.html) dan pada contoh diatas saya menggunakan `GlobalScope` yang berarti lifecycle coroutinenya mengikuti lifecycle seluruh aplikasi.

pada no `2` saya memanggil `suspending function` untuk melakukan delay selama 2sec, pada no `3` saya outputkan text, jika program dijalankan maka yang akan muncul pertama kali adalah kode no `4` hal ini karena kode 1 dijalankan pada coroutine, apabila kita tidak menuliskan Thread.sleep(2000) yang mana maksudnya adalah untuk mempause thread selama 2 sec, maka program tidak akan pernah menampilkan kata `Dunia . . .`

<iframe src="https://pl.kotl.in/nkYEw9175" width="500"></iframe>

Terdapat 2 cara untuk menjalankan coroutine :
- dengan fungsi `launch {}`
- dengan fungsi `async {}`

perbedaan launch dan async adalah, `launch` tidak mengembalikan data sedangkan `async` akan mengembalikan object dari `Deffered<T>` yang mana akan memiliki fungsi `await()`