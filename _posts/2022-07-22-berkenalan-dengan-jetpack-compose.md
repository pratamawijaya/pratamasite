---
layout: single
title:  "Berkenalan dengan Jetpack Compose"
date:   2022-07-23 09:00:00 +0700
excerpt: "Berkenalan dengan jetpack compose, apa itu Jetpack Compose cara menggunakannya dll akan dibahas pada artikel ini"
categories:
- android
tags :
- android
- programming
- kotlin
- jetpack-compose
---

Untuk membuat user interface pada sebuah aplikasi android kita biasanya akan menggunakan syntax xml sebagaimana yang dikenalkan 
oleh Google sejak framework android muncul, namun pada akhir-akhir ini Google mengeluarkan sebuah cara baru untuk membuat user interface android yaitu 
`Jetpack Compose`. Pada artikel ini akan membahas mengenai `Jetpack Compose` dari perspektif Android View XML yang sebelumnya ada.


## Dari View berubah menjadi @Composable

Pada `Jetpack Compose` setiap component yang akan dirender pada screen, akan dibuat menjadi sebuah Kotlin Unit
function dan diberi tanda anotasi `@Composable` sebagi berikut


````java
@Composable
fun Article(title: String, desc: String){
    Card {
        Column {
            Text(title)
            Spacer(Modifier.height(10.dp)
            Text(desc)
        }
    }
}
````

Fungsi diatas disebut sebagai ***composable***, contoh fungsi diatas akan menampilkan Card dengan
title dan description, dengan jarak diantaranya 10.dp.

Setiap title dan description berubah, UI akan terupdate sesuai dengan nilainya. Proses ini pada Jetpack
Compose disebut dengan `recomposition`.

Sebuah composable function hanya bisa dipanggil dari fungsi composable lain. Jika ingin
memanggil sebuah composable function pada Activity, kita bisa memanggilnya seperti ini :

````java
class SuperActvity : ComponentActivity() {
     override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            // composables function dipanggil disini
        }
    }
}
````

Contoh diatas kita dapat memanggil composables function didalam method `setContent {}` yang mana
method ini berasal dari class `ComponentActivity`, sedangkan untuk Fragment kita membutuhkan
`ComposeView`, contoh :


````java
class MyFragment : Fragment() {
    override fun onCreateView(
        ....
    ): View {
        return ComposeView(requireContext()).apply {
            setViewCompositionStrategy(DisposeOnViewTreeLifecycleDestroyed)
            setContent {
                // composables function dipanggil disini
            }
        }
    }
}

````