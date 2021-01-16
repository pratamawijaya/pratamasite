---
layout: single
title:  "Mengatasi Kotlin Android Extensions Deprecated dengan Android ViewBinding"
date:   2021-01-16 09:00:00 +0700
excerpt: "Update Kotlin Android Extensions Deprecated dengan Android ViewBinding"
categories: 
    - android
tags : 
    - android
    - programming
    - kotlin
    - view
--- 

Kotlin Android Extensions merupakan sebuah plugins yang sudah lama saya gunakan, semenjak aktif menggunakan Kotlin untuk membuat aplikasi android saya selalu menggunakan plugin ini, tujuannya adalah untuk memudahkan inisiasi sebuah View pada aplikasi android.

Namun pada akhir tahun lalu, Google mengumumkan bahwa Kotlin Android Extensions ini statusnya menjadi **Deprecated** dan akan digantikan oleh `ViewBinding`, hal ini dimulai sejak update android studio 4.1 keatas (versi kotlin 1.4.20), bila kalian masih menggunakan KotlinAndroidExtensions dan kalian akan melihat pesan error seperti ini

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated.png){:class="img-responsive"}

Penjelasan lengkap kenapa kotlin android extensions deprecated bisa dibaca di blognya Google berikut ini https://android-developers.googleblog.com/2020/11/the-future-of-kotlin-android-extensions.html

Lalu penggantinya adalah ViewBinding, owh ya sebelum lebih lanjut ViewBinding ini tidak sama dengan DataBinding yang sebelumnya telah diperkenalkan, untuk menggunakan ViewBinding, caranya mudah berikut langkah-langkahnya 


# Update build.gradle
buka file app/build.gradle lalu tambahkan kode berikut

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_1.png){:class="img-responsive"}

# Contoh penggunaan pada Activity

semisal kita punya file layout seperti ini

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btn_satu"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn_dua"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/btn_satu" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

dari layout diatas, ViewBinding akan mengenerate file bernama `ActivityMainBinding`

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_2.png){:class="img-responsive"}

setiap file layout xml akan digenerate dengan `Pascal Format` dengan tambahan kata `Binding` dibelakangnya, selain itu field namenya juga akan digenerate dengan `CamelCase Format` misal sebelumnya idnya adalah btn_dua setelah digenerate akan menjadi btnDua

lalu untuk menggunakannya didalam Activity kita perlu menulis kode seperti berikut

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_3.png){:class="img-responsive"}


selanjutnya untuk mengakses field yang ada pada layout kita bisa menulis seperti ini

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_4.png){:class="img-responsive"}


# Contoh penggunaan pada Fragment

lalu bagaimana jika digunakan pada fragment ?
contoh, kita punya layout fragment seperti ini

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SampleFragment">


    <EditText
        android:id="@+id/input_name"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="90dp"
        android:hint="Input Name" />

    <Button
        android:id="@+id/btn_hitme"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/input_name"
        android:layout_centerHorizontal="true"
        android:text="Hit Me" />

</RelativeLayout>
```

kita bisa akses seperti ini

![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_5.png){:class="img-responsive"}


![kotlin-android-extensions-deprecated](/assets/images/kotlin_android_extensions/kotlin_android_extensions_deprecated_6.png){:class="img-responsive"}

