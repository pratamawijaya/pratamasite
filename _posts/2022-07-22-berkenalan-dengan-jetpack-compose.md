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


````
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

## Listener dan Atribute dengan Modifiers

Ketika menggunakan view, sudah umum kita akan menemukan atribute-atribute yang biasa ada pada view
misalkan, click dan touch listener, evelation, alpha, dll. Jetpack compose mengenalkan konsep `Modifier`
yang mana feature ini memberikan fungsionality pada ***composable*** tanpa harus terikat dengan 
composables lainnya, contoh modifier yang dapat digunakan untuk mengubah style composable adalah `background()`, `border()`, `clip()`
`shadow()`, `alpha()`. sedangkan untuk mengubah posisi dan ukuran composables bisa menggunakan
`fillMaxWidth()`, `size()`, `heightIn()`, `padding()` lalu modifier yang bisa digunakan untuk menambah 
functionality sebuah composables seperti click, dragging contohnya `clickable()`, `draggable()`, `toggleable()`,`swipeable()`

Untuk dokumentasi lengkap fungsi `Modifier` dapat dicek pada [halaman berikut](https://developer.android.com/jetpack/compose/modifiers-list)

## Dari ViewGroups menjadi Rows, Column, Box, LazyColumn, LazyRow

Untuk membuat sebuah UI pada Jetpack Compose kita bisa memulai dari composables function
yang biasa digunakan seperti `Column`, `Row`, `Box`, `LazyColumn`, `LazyRow`. Fungsi-fungsi 
composables ini memiliki parameter yang dapat menerima fungsi composables lain.

Untuk mengatur aligment Horizontal ataupun Vertical, kita bisa menggunakan `Column` atau `Row`. Selain itu jika sebelumnya
terbiasa dengan atribute `layout_weight` yang ada diLinearLayout, pada Jetpack Compose juga memiliki fungsi yang sama yaitu
modifier `weight()`, contoh penggunaan

````
Row {
    Text("Judul")
    Spacer(Modifier.weight(1f)
    Text("1 jam yang lalu")
}
````

Sedangkan `Box` pada Jetpack Compose ini merupakan pengganti untuk `FrameLayout`, dengan `Box` kita 
bisa mengatur posisi setiap composables satu dengan lainnya menggunakan modifier `align` sama halnya ketika menggunakan
FrameLayout dengan atribute gravity nya, contoh pada Jetpack Compose

````
Box {
    Image(
        painter = painterResource(R.drawable.some_image)
    )
    Text(
        "Desc"
        modifier = Modifier
            .padding(4.dp)
            .align(Alignment.BottomEnd),
    )
}
````

Untuk `Recyclerview` pada Jetpack Compose diganti dengan `LazyColumn` dan `LazyRow`, column ini untuk vertical scrolling, 
sedangkan row untuk yang horizontal scrolling, contoh 

````
val data = listOf("")

LazyColumn(Modifier.fillMaxSize()) {

    items(data) { item ->
        Text(
                item,
                modifier = Modifier.padding(vertical = 10.dp, horizontal = 8.dp)
            )
    }
}

````