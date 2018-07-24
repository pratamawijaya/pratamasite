---
layout: single
title:  "Tutorial Android : Membuat Gridlayout Recyclerview dengan Kotlin"
date:   2018-07-24 10:00:00 +0700
excerpt: "Membuat gridlayout dengan recyclerview dan menggunakan bahasa kotlin"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
    - recyclerview
---

Pada post [sebelumnya](https://pratamawijaya.com/programming/android-recyclerview-kotlin/) telah saya bahas bagaimana membuat recyclerview dengan bahasa kotlin, kali ini saya akan bahas bagaimana jika kita butuh tampilan recylerview tersebut berbentuk grid ? jawabannya tinggal ubah bagian **layoutmanager** nya, 

salah satu kelebihan recyclerview dibanding listview adalah fleksibel dalam menentukan layout manager,

misal kita mau menampilkan list datanya secara horizontal bukan vertical, tinggal ubah layout manager menjadi

```java
rvMain.apply {
            layoutManager = LinearLayoutManager(this@MainActivity, LinearLayoutManager.HORIZONTAL, false)
            adapter = heroesAdapter
        }

```
- this@MainActivity : digunakan untuk mengambil context app
- LinearLayoutManager.HORIZONTAL: untuk menetukan orientasi menjadi horizontal
- false : parameter untuk menentukan apakah data perlu dibalik(reverse) atau tidak

setelah dijalankan akan menjadi seperti ini

![Android RecyclerView](/assets/images/recyclerview/rv_3.png){:class="img-responsive"}

jika diperhatikan mungkin kalian akan menganggap ini error karena data hanya tampil satu, namun sebenarnya ketika discrool kekiri akan nampak data selanjutnya, hal ini terjadi karena dari layout item hero, masih belum dirubah, layout parentnya masih match_parent dimana akan mengikuti besaran layar device, maka dari itu layout item hero dirubah menjadi seperti ini :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:orientation="vertical"
              android:padding="8dp">

    <ImageView
        android:id="@+id/imgHeroes"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:scaleType="centerCrop"/>

    <TextView
        android:id="@+id/txtHeroName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        />

</LinearLayout>
```

![Android RecyclerView](/assets/images/recyclerview/rv_4.png){:class="img-responsive"}

lalu bagaimana jika bentuknya ingin grid, bukan menyamping horizontal seperti itu, caranya yaitu menggunakan **GridlayoutManager**

```java
 rvMain.apply {
            layoutManager = GridLayoutManager(this@MainActivity, 3)
            adapter = heroesAdapter
        }
```

Gridlayout ini memiliki dua paramater
- parameter pertama untuk context
- parameter kedua untuk menentukan berapa banyak kolom

hasil akhir ketika dijalankan akan menjadi seperti ini

![Android RecyclerView](/assets/images/recyclerview/rv_5.png){:class="img-responsive"}

cukup mudah bukan ? semoga bermanfaat buat kalian semua.
