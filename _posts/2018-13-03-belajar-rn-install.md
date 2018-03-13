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
