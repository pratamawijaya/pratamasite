---
layout: single
title:  "Mengenal Dependency Injection"
date:   2017-10-02 11:30:37 +0700
excerpt: "Mengenal dependency injection pada java programming"
categories: 
    - programming
tags : 
    - java 
    - konsep
    - basic
    - android
    - di
---
Dagger merupakan library yang popular digunakan oleh professional android developer di dunia. Jargon yang sering didengar ketika
menggunakan dagger adalah _Dependency Injection_ sebelum menggunakan dagger ke project android anda ada baiknya memahami maksud
_Dependency Injection_ itu sendiri.

Jadi apakah _Dependency Injection_ itu ?

> Dependency Inversion Principle is simply a guideline to write loosely-coupled code.

_loosely coupled_ disini adalah konsep untuk mengurangi ketergantungan (interdependency) dari suatu sistem, jadi meminimalisir
suatu class agar tidak terikat dengan class lain.

# Contoh Kasus
contoh kasus kali ini anggap saja saya memiliki service untuk mengirim email, classnya adalah sebagai berikut
```java
    public class EmailService {
        public void sendEmail(String message, String receiver) {
            // logic untuk mengirim email
            Log.d("tag"," email dikirm ke " + receiver);
        }
    }
```

class **EmailService** ini menyimpan logika untuk mengirim email ke alamat email yang dituju, kemudian untuk main class 
nya yang bertugas menjalankan service tersebut kurang lebih seperti ini

```java
    public class MyPresenter {
        private EmailService email = new EmailService();

        public void processMessages(String msg, String to) {
            this.email.sendEmail(msg, to);
        }
    }
```
secara kasat mata tidak ada yang salah dengan implementasi kode diatas, namun logic code diatas memiliki  kekurangan.

**MyPresenter** class bertugas untuk initialize email service dan menggunakannya,padahal email service ini bisa saja digunakan di banyak class, hal ini berujung ke _hardcoded_ dependency, jika kedepannya akan ada perubahan email service maka harus ada perubahan di class **MyPresenter** dan class lainnya yang menggunakan class **EmailService** ini, hal ini menyulitkan aplikasi untuk di extend, dan apabila email service digunakan di banyak class hal ini akan lebih mempersulit.

Untuk mengatasi hal tersebut maka kita bisa menggunakan prinsip DI.

# DI Manual
Dengan menggunakan prinsip DI maka class **MyPresenter** tidak lagi mengurusi inisiasi **EmailService**, presenter tersebut
hanya menerima object dari class service lalu menggunakannya. Lalu bagaimana memasukkan class service tersebut kedalam presenter ? salah satu caranya adalah melalu constructor.

```java
    public class MyPresenter {
        private EmailService email;

        public MyPresenter(EmailServices services) {
            this.email = services;
        }

        public void processMessages(String msg, String to) {
            this.email.sendEmail(msg, to);
        }
    }
```

lalu di activity/fragmentnya kita bisa menggunakannya seperti ini
```java
    public class SomeActivity extends AppCompat {

        private MyPresenter presenter;
        private EmailServices services;

        @Override
        public void onCreate() {
            services = new EmailServices();

            // pass services ke presenter
            presenter = new MyPresenter(services);
        }
    }
```