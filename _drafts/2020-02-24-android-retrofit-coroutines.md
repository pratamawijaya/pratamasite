---
layout: single
title:  "Tutorial android : Retrofit dan Coroutines"
date:   2020-02-24 09:00:00 +0700
excerpt: "Tutorial penggunaan retrofit dengan menggunakan coroutines sebagai library async"
categories: 
    - android
tags : 
    - android
    - programming
    - kotlin
    - coroutines
--- 

Pada artikel ini saya akan membahas bagaimana caranya menggunakan coroutines dan retrofit secara bersamaan, dengan menggunakan coroutines apa yang kita dapatkan ? kita tidak perlu lagi menggunakan **callback** 

sebelum menggunakan coroutines
![coroutines-retrofit](/assets/images/coroutines/retrofit_vanilla.png){:class="img-responsive"}

sesudah menggunakan coroutines
![coroutines-retrofit](/assets/images/coroutines/retrofit_coroutines.png){:class="img-responsive"}

Pada contoh ini saya akan coba buat apps sederhana dengan flow diagram sebagai berikut

![coroutines-retrofit](/assets/images/coroutines/diagram_flow_retrofit.png){:class="img-responsive"}

hal pertama yang perlu ditambahkan adalah dependency coroutines dan retrofit, tambahkan baris berikut pada file app/build.gradle

![coroutines-retrofit](/assets/images/coroutines/retrofit_dep.png){:class="img-responsive"}

selanjutnya untuk **api**, saya menggunakan api https://jsonplaceholder.typicode.com/todos/1 untuk demo, saya simpan dalam sebuah file bernama Webservices.kt

![coroutines-retrofit](/assets/images/coroutines/retrofit_webservices.png){:class="img-responsive"}

kemudian untuk class repository saya buat seperti ini

![coroutines-retrofit](/assets/images/coroutines/retrofit_repository.png){:class="img-responsive"}

selanjutnya tinggal memanggil class repository tersebut dari activity atau fragmentnya, pertama kita harus implementasi **CoroutineScope** terlebih dahulu, agar bisa menggunakan builder **launch** untuk coroutines,

![coroutines-retrofit](/assets/images/coroutines/retrofit_coroutinesscope.png){:class="img-responsive"}

selanjutnya untuk pemanggilan repositorynya kita perlu memanggilnya didalam builder launch seperti ini

![coroutines-retrofit](/assets/images/coroutines/retrofit_launch_retro.png){:class="img-responsive"}

withContext(Dispatchers.IO) disini digunakan untuk memastikan bahwa repository.getTodo dijalankan di coroutine dengan context Dispatchers.IO

contoh kode lengkap dapat diakses pada repo berikut [ini](https://github.com/pratamawijaya/SimpleCoroutinesRetrofit)


## Basic Coroutines
- [Apa itu coroutines](https://pratamawijaya.com/android/apa-itu-coroutines/)