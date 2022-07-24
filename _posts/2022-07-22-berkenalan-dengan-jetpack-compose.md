---
layout: single
title:  "Berkenalan dengan Jetpack Compose"
date:   2022-07-23 09:00:00 +0700
excerpt: "Artikel ini membahas mengenai Jetpack Compose, sebuah tools baru dari Android Teams untuk membangun User Interface pada sebuah aplikasi android"
categories:
- android
tags :
- android
- programming
- kotlin
- jetpack-compose
---

Pada pengembangan sebuah aplikasi android untuk membuat user interfacenya kita biasanya akan menggunakan syntax xml sebagaimana yang dikenalkan 
oleh Google sejak framework android muncul, namun pada akhir ini Google mengeluarkan sebuah cara baru untuk membuat user interface android yaitu 
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

Kemudian untuk mengatur aligment Horizontal ataupun Vertical, kita bisa menggunakan `Column` atau `Row`. Selain itu jika sebelumnya
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

`Recyclerview` pada Jetpack Compose diganti dengan `LazyColumn` dan `LazyRow`, column ini untuk vertical scrolling, 
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

## Handling business logic pada Composables function

Sebuah composables function akan selalu merepresentasikan sebuah element pada screen (contoh avatar) atau bisa juga merepresentasikan sebuah halaman utuh, pada `Jetpack Compose` ada sebuah istilah yang dinamakan (`state hoisting`)[https://developer.android.com/jetpack/compose/state#state-hoisting] sebuah pattern untuk handle state pada sebuah composables yang mana tujuannya adalah agar sebuah function composables menjadi `stateless`

Umumnya pattern ini akan selalu memiliki :

- `value: T` : nilai yang akan ditampilkan pada composables
- `onValueChange: (T) -> Unit` : sebuah event yang akan digunakan untuk mengubah value, dimana T adalah value baru yang akan disimpan 

````
@Composable
fun HelloScreen() {
    var name by rememberSaveable { mutableStateOf("") }

    HelloContent(name = name, onNameChange = { name = it })
}

@Composable
fun HelloContent(name: String, onNameChange: (String) -> Unit) {
    Column(modifier = Modifier.padding(16.dp)) {
        Text(
            text = "Hello, $name",
            modifier = Modifier.padding(bottom = 8.dp),
            style = MaterialTheme.typography.h5
        )
        OutlinedTextField(
            value = name,
            onValueChange = onNameChange,
            label = { Text("Name") }
        )
    }
} 
````

Pada contoh diatas, composables `HelloContent` memiliki 2 parameter, 
- name : sebagai value
- onNameChange: event untuk mengubah value name

Pada kode diatas, composables `HelloContent` sama sekali tidak akan menyimpan data pada scope functionsya, function HelloContent sudah menjadi sebuah stateless function, setiap perubahan data nama akan dibawa ke-atas (`hosting`) yakni ke composables `HelloScreen`


## Cheatsheet

![Jetpack Compose Cheatsheet](assets/images/jetpack-compose/compose-cheat-sheet.webp)



***References***
- https://www.composables.co/blog/compose-intro 
- https://developer.android.com/jetpack/compose/documentation