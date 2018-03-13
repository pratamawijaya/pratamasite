---
layout: single
title:  "Belajar React Native 1: Install React Native Error Build"
date:   2018-03-13 16:30:21 +0700
excerpt: "Belajar React Native Part 1"
categories: 
    - Programming
tags : 
    - programming
    - android
    - react-native
---

Halo semuanya, jumpa lagi dengan artikel saya, kali ini saya akan share mengenai react-native, ya kalian tidak salah dengar, pada tutor pertama ini saya akan menulis tentang problem saya ketika melakukan instalasi atau memulai project pertama react-native.

Seperti kebanyakan framework lainnya, react-native juga telah menyediakan dokumentasi pada halaman berikut 

https://facebook.github.io/react-native/docs/getting-started.html

untuk proses insttalasi react-native-cli saya tidak menemukan kendala yang berarti, hanya saja versi npm di laptop saya yang harus di downgrade ke versi 4, untuk saat artikel ini ditulis react-native belum support npm versi 5 keatas. Namun masalah baru saya temukan ketika akan mencoba running projectnya ke android emulator

[![asciicast](https://asciinema.org/a/kpPCrzxs4LYciOjHKZORH6hbI.png)](https://asciinema.org/a/kpPCrzxs4LYciOjHKZORH6hbI)

errornya adalah

```java
java.lang.NullPointerException (no error message)   
```

setelah saya coba analisa dengan cara paste error message tersebut ke google dengan menambahkan kata kunci react-native ternyata problemnya adalah dikarenakan versi gradle tools yang masih menggunakan versi 2 dan gradle wrapper yang masih menggunakan versi lama, step by step fixnya adalah

buka folder file gradle-wrapper.properties di folder android/app/gradle/wrapper kemudian ganti menjadi seperti ini

```bash
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip

```

langkah kedua buka file build.gradle yang berada di folder android/build.gradle dan rubah menjadi seperti ini

```bash
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}

```

kemudian setelah dirubah coba compile ulang, maka hasilnya adalah seperti dibawah ini

[![asciicast](https://asciinema.org/a/MtNLpo3gMVQR1R4cJHHK9qKhX.png)](https://asciinema.org/a/MtNLpo3gMVQR1R4cJHHK9qKhX)

Sekian cerita pengalaman saya yang baru mencoba pertama kali react-native.

Salam