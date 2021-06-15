---
layout: single
title:  "Apa itu Corutines Dispatchers ?"
date:   2021-06-15 09:00:00 +0700
excerpt: "Mengenal Kotlin Coroutines Dispatchers"
categories: 
    - android
tags : 
    - android
    - programming
    - kotlin
    - coroutines
---

Artikel kali ini kelanjutan dari pembahasan sebelumnya terkait **Coroutines**, kali ini saya akan coba membahas mengenai 
**Dispatchers**, dengan Dispatcher kita bisa menentukan Thread yang akan digunakan untuk menjalankan Coroutine. Berikut ini adalah list Dispatchers yang ada pada Kotlin Coroutines :

## Dispatchers.Default

**Dispatcher** ini akan digunakan ketika kita memanggil sebuah coroutine tanpa menentukan dispatcher yang akan digunakan, dispatcher ini menggunakan shared pool thread yang ada pada JVM, dan secara default jumlah maximum threadnya sama dengan jumlah core processor. Biasa digunakan untuk task yang berkaitan dengan computation.

## Dispatchers.IO

**Dispatcher** yang didesain untuk task yang bersifat IO, dispatcher ini juga menggunakan shared pool thread, dengan catatan jumlah thread yang digunakan untuk task pada dispatcher ini dilimit sesuai dengan value yang ada pada “kotlinx.coroutines.io.parallelism”, by default akan dilimit pada 64 thread atau jumlah core processor (yang paling besar yang akan digunakan).

## Dispatchers.Main

**Dispatcher** yang didesain untuk digunakan main thread (proses UI), dan biasanya berupa single thread, untuk android sendiri selain ada **Dispatcher.Main** ada juga Dispatcher.Main.immediate, bedanya dengan dispatcher.main biasa adalah coroutine function yang menggunakan dispatcher.main.immediate ini akan jalan duluan dibanding dengan coroutines function yang menggunakan dispatcher main.

contoh :

![Dispatchers.Main.Immediate](/assets/images/coroutines/dispatchermain_immediate.png)

hasilnya akan print `Third statement` terlebih dahulu, padahal kode untuk print first statement ada diatas kode third statement

## Dispatchers.Unconfined

**Dispatcher** ini agak beda dengan dispatchers lainnya, jika menggunakan dispatchers ini, coroutine function kita akan dijalankan langsung ke Thread yang saat ini sedang digunakan. Biasanya digunakan ketika sedang melakukan testing.


## Kesimpulan 
- untuk menjalankan code pada UI Thread, gunakan Dispatcher.Main.Immediate
- untuk background thread bisa menggunakan Dispatcher.Default atau Dispatcher.IO
- dispatcher.unconfined tidak digunakan pada android apps, tidak ada usecase yang cocok

Jadi itulah artikel mengenai Coroutine Dispatchers, semoga bermanfaat.
Thanks