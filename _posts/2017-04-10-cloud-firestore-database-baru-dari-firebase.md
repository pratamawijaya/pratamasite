---
layout: single
title:  "Cloud Firestore NoSql Document Database dari Firebase"
date:   2017-10-04 12:10:37 +0700
excerpt: "Mengenal Cloud Firestore NoSql Document Database dari Firebase yang baru saja diluncurkan"
categories: 
    - news
tags : 
    - firebase
    - database
    - nosql
    - firestor
---

![Firestore Database](/assets/images/cloud_firestore_firebase.png){:class="img-responsive"}

Beberapa waktu lalu google mengumumkan peluncuran jenis database baru untuk Firebase, firebase sendiri adalah Backend as a Service untuk app developers. Firestore sendiri menurut google diluncurkan untuk menyempurnakan firebase realtime database yang ada saat ini.

Firebase realtime database sendiri adalah product yang sering digunakan oleh developer karena memiliki fitur yang sangat membantu mulai dari _realtime database_, offline support, security role.

Sedangkan firestore memiliki fitur yaitu : 
- _Documents__ dan _collections_ dengan query yang lebih powerfull
- dukungan terhadap sdk untuk iOS, Android dan Web
- Real-time data sync
- Automatic, multi region data replication 
- SDK untuk bahasa pemrograman server side (Node, Python, Go, Java)

<iframe width="560" height="315" src="https://www.youtube.com/embed/QcsAb2RR52c" frameborder="0" allowfullscreen></iframe>

## Apa yang membedakan Cloud Firestore ?
Jika kalian sebagai developer mungkin memiliki pertanyaan ini, jadi apa bedanya dengan realtime database sebelumnya ?

sebelumnya saya sendiri juga pengguna realtime database firebase, lalu muncul firestore ini, keduanya termasuk NoSql Database [sumber](https://firebase.google.com/docs/firestore/rtdb-vs-firestore)

### Realtime Database

menyimpan data dalam bentuk Json Tree

![Realtime Database](/assets/images/realtime_db.png){:class="img-responsive"}

* pros : data yang simple sangat mudah untuk disimpan
* cons : komplektisitas, data hirarkis sulit diatur ketika akan melakukan _scaling_

### Cloud Firestore
menyimpan data dalam dokumen yang diatur didalam collection

![Firestore Database](/assets/images/firestore_database.png){:class="img-responsive"}

- data simple masih mudah untuk disimpan
- complex data dan hierarchial data lebih mudah di scale karena penggunaan _collections_
- mengurangi _denormalization_ data dan data _flattening_

## Data Type
firestore via webconsole sudah menyediakan list data type apa saja yang disupport, jadi meminimalisir kesalahan yang terjadi ketika salah mendefinisikan data type untuk sebuah field

![Firestore Data Type](/assets/images/firestore_datatype.png){:class="img-responsive"}

## Query 

### Realtime Database
kemampuan query masih dibatasi fungsi sorting dan limitasi

- query realtime database memilki kelemahan dimana ketika query satu node dan hanya mengambil 1 data, maka yang diambil adalah seluruh data dari node tersebut terlebih dahulu kemudian dilakukan sorting.

### Cloud Firestore
query sudah support indexing, dan sorting filtering yang lebih baik


saat ini status dari cloud firestore sendiri masih beta, saya sendiri baru membaca dan mencoba memahami seperti apa firestore tersebut, mungkin kedepannya akan saya coba buat project dengan menggunakan cloud firestore sebagai databasenya,

sekian ulasan singkat dari saya tentang cloud firestore, terimakasih