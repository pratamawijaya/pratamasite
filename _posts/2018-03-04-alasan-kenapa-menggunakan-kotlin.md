---
layout: single
title:  "5 alasan kenapa kamu harus mencoba menggunakan kotlin"
date:   2018-04-03 12:10:21 +0700
excerpt: "5 alasan buat kamu kenapa harus mencoba kotlin"
categories: 
    - Programming
tags : 
    - programming
    - android
    - kotlin
---

Hi semuanya kali ini saya akan membahas mengenai sebuah bahasa yang saat ini sering saya gunakan yaitu Kotlin. Kenal kotlin semenjak Jake Wharton mengeluarkan dokumen **Using Project Kotlin for Android** pada tahun 2015 [link](https://docs.google.com/document/d/1ReS3ep-hjxWA8kZi0YqDbEhCqTt29hG8P44aA9W0DM8/edit#heading=h.96ldte2znfpc) 

![Kotlin for Android](/assets/images/jake_kotlin_android.png){:class="img-responsive"}

pada saat itu saya hanyalah pada state membaca dan memahami apa itu kotlin, hingga akhirnya pada akhir 2015 atau awal 2016 ada salah satu plugin gradle yang dibuat senior saya yang menggunakan kotlin sebagai bahasa pemrogramannya, saat itu plugin yang dibuat memiliki fungsi untuk upload apk file ke amazon s3, saat itu versi kotlin masih 1.0.0-beta dan dari situ pula banyak mendapatkan pelajaran yang menarik.

Lalu akhir2 ini saya perhatikan banyak sekali yang bertanya, kenapa menggunakan kotlin ? apa bedanya dengan java ? mari kita bahas

## Interoperable
Maksud dari interoperable adalah kode yang ditulis dengan bahasa kotlin dapat dipanggil di class yang ditulis dengan bahasa java, begitu juga sebaliknya class java bisa dipanggil juga di class yang ditulis dengan kotlin.

## Data Class
Berapa baris sebuah pojo (Plain Old Java Object) yang biasa kalian buat ? 

contoh java

```java
package com.pratama.kotlindemogradle;

public class UserJava {
    private String nama;
    private int umur;

    public UserJava(String nama, int umur) {
        this.nama = nama;
        this.umur = umur;
    }

    public String getNama() {
        return nama;
    }

    public void setNama(String nama) {
        this.nama = nama;
    }

    public int getUmur() {
        return umur;
    }

    public void setUmur(int umur) {
        this.umur = umur;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        UserJava userJava = (UserJava) o;

        if (umur != userJava.umur) return false;
        return nama != null ? nama.equals(userJava.nama) : userJava.nama == null;
    }

    @Override
    public int hashCode() {
        int result = nama != null ? nama.hashCode() : 0;
        result = 31 * result + umur;
        return result;
    }

    @Override
    public String toString() {
        return "UserJava{" +
                "nama='" + nama + '\'' +
                ", umur=" + umur +
                '}';
    }
}

```

contoh kotlin

```java
data class UserKotlin(val name: String, val umur: Int)
```

dengan sebaris kode pojo diatas, kotlin sudah menambahkan fungsi toString, equals, hashcode.
Secara default value pada Data Class adalah immutable, untuk merubah suatu value dari data class bisa menggunakan method copy dan memindahkannya ke object lain

```java
val user = UserKotlin("Pratama", 27)
val anotherUser = user.copy(umur = 30)
```

![Data Class](/assets/images/data_class.png){:class="img-responsive"}


## Null Safety
Nullpointer adalah error yang cukup umum terjadi di bahasa java, pada bahasa kotlin null variable mendapatkan perlakuan khusus, secara default setiap variable yang dideklarasikan dikotlin tidak dapat menerima nilai null (non-null) 
contoh :

```java
val someData:String = null // tidak bisa dicompile, error 

```

![Non Null Value](/assets/images/non_null.png){:class="img-responsive"}

pada dasarnya dalam programming kita tidak bisa menghindari sebuah null variable, maka dari itu kotlin memiliki fitur _null safety_

contoh penerapan null safety

```java

val someData:String? = null // bisa dicompile karena variable someData menjadi nullable
```

ketika sebuah variable dideklarasikan _nullable_ namun kita tidak menghandle nullable ini maka compiler akan memberitahukan error, contoh

![Non Null Value](/assets/images/nullable_unhandle.png){:class="img-responsive"}

untuk menangani data _nullable_ seperti ini bisa menggunakan beberapa cara, 
cara pertama

```java

    if (someData != null) {
        println(someData.length)
    }
```
sama halnya dengan handling null value di java

atau cara kedua yang lebih kotlin banget

```java
println(someData?.length)
```
cara kedua adalah cara yang paling direkomendasikan,
kita juga bisa memberikan  value ketika sebuah variable null dengan cara menggunakan _elvis operator_

```java
    val x = someData?.length ?: -1
```

maksud dari kode diatas adalah, jika panjang somedata tidak null maka nilai x adalah panjang someData, jika someData null maka nilai x adalah -1

# Extension Function
Fitur ini adalah kemampuan untuk membuat sebuah function baru yang terikat kesuatu class tanpa kita harus melakukan inheritance ke class tersebut.

contoh:

```java
fun String.last(): Char {
    return this[length - 1]
}

```
kode diatas, saya membuat sebuah fungsi bernama last yang saya tambahkan langsung ke class String, maka saya langsung bisa mengaksesnya kapanpun saya mau, 

```java

println("Kotlin Indonesia".last())

```

![Extension Function](/assets/images/extension_function.png){:class="img-responsive"}

## Functional
Bagi yang terbiasa dengan rx dan functional programming, mungkin sudah tidak asing lagi dengan hal semacam ini,

contoh:

```java
val listUser = listOf(
            UserKotlin("beta", 12),
            UserKotlin("yoga", 15),
            UserKotlin("tama", 18)
    )

val filteredUser = listUser.filter { it.umur >= 15 }.map { "${it.name} user" }
    filteredUser.map { println(it) }
```

contoh diatas adalah filter data dengan umur >= 15 kemudian outputkan dengan nama user + user

## Karena sudah ada Kursus Kotlin Android versi bahasa Indonesia

Buat kalian yang berminat untuk mengambil kelas kursus kotlin android berbahasa Indonesia bisa kesini [http://bit.ly/course_kotlin_android](http://bit.ly/course_kotlin_android)