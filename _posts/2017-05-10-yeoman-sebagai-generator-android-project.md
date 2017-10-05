---
layout: single
title:  "Yeoman sebagai generator android project"
date:   2017-10-05 08:30:21 +0700
excerpt: "Mudah dan cepat membuat base android project dengan yeoman"
categories: 
    - Programming
tags : 
    - tips
    - generator
    - yeoman
    - android
---

![Yeoman Logo](/assets/images/yeoman-logo.png){:class="img-responsive"}

Yeoman adalah _Web Scaffolding Tool_ yang dapat digunakan untuk mengenerate project berbasis web. Yeoman disini berperan untuk mempercepat proses development sebuah web, dengan mempersiapkan kebutuhan-kebutuhan dasar dari web app misal library-library, base code yang perlu disiapkan diawal.
Dengan menggunakan yeoman developer akan terbantu dalam mempersiapkan project, sehingga developer akan lebih memiliki banyak waktu untuk berpikir di bisnis proses appnya.

Setelah mengenal yeoman sebagain generator web app, sempat terpikir bisa tidak yeoman ini digunakan juga untuk mengenerate android base project, jadi waktu untuk mempersiapkan project basenya lebih cepat, jawabannya adalah bisa.

![Android Generator](/assets/images/generator-android.png){:class="img-responsive"}

untuk dapat menggunakan android generator tersebut ada beberapa yang perlu di install, yang pertama adalah **npm** jika npm sudah terinstall selanjutnya adalah menginstall yeoman 

```bash
    npm install -g yo
```

kemudian install juga untuk generatornya
```bash
    npm install -g generator-androidstarters
```

<script type="text/javascript" src="https://asciinema.org/a/oAzXdXjRs0NP04eBPmxY3JjJq.js" id="asciicast-oAzXdXjRs0NP04eBPmxY3JjJq" async></script>

sangat mudah dan cepat, kita juga diberikan pilihan untuk menggunakan base project yang telah disediakan oleh androidstarters, selain itu ada juga versi webnya

![Android Generator](/assets/images/android_starter_generator.png){:class="img-responsive"}

kemudian tinggal download dan extract file yang ada selanjutnya dibuka menggunakan android studio.

Lalu, bisa tidak buat generator dengan base code kita sendiri ? jawabannya bisa, kemarin saya buat ini [Generator Base Android Kotlin](https://github.com/pratamawijaya/generator-base-android-kotlin) yang dibantu rekan saya [Esa Firman](http://nolambda.stream)