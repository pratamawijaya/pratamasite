---
layout: single
title:  "Tutorial Android : Membuat Recyclerview secara simple menggunakan Groupie"
date:   2018-07-26 09:00:00 +0700
excerpt: "Membuat list dengan recyclerview secara simple dengan library gropie"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
    - recyclerview
---

Setelah pada artikel-artikel sebelumnya kita belajar membuat [recyclerview di android](https://pratamawijaya.com/programming/android-recyclerview-kotlin/), kali ini saya akan membahas bagaimana cara membuat recyclerview lebih simple dan cepat, cepat disini maksudnya adalah proses membuatnya tidak lama, kenapa saya bilang lama ? karena untuk membuat sebuah recyclerview kita melalui beberapa tahapan sebagai berikut :

- membuat recyclerview
- membuat adapter
- membuat holder
- membuat layout item

Salah satu cara agar ketika membuat recyclerview lebih simple dan cepat adalah dengan menggunakan library, salah satunya adalah [Groupie](https://github.com/lisawray/groupie).

Groupie adalah library yang membantu kita untuk menghandle recyclerview yang kompleks, cara menggunakan library ini, langkah2nya adalah :

## Menambahkan Dependency

tambahkan 

```java
implementation 'com.xwray:groupie:2.1.0'
```

selain itu groupie juga memiliki module untuk kotlin dan kotlin android extension, yang membuat kalian tidak akan perlu menulis ViewHolder lagi, cara setupnya tinggal tambahkan 

```java
implementation 'com.xwray:groupie-kotlin-android-extensions:2.1.0'
```

## Deklarasi adapter

langkah pertama kita perlu mendeklarasi **GroupAdapter**, groupadapter ini sebenarnya adalah RecyclerView.Adapter yang langsung bisa kalian gunakan ke RecyclerView kalian

```java
val groupAdapter = GroupAdapter<ViewHolder>()
recyclerview.adapter = groupadapter
```

## Deklarasi Item

lalu selanjutnya buat class untuk assign value ke tiap itemnya

```java
class HeroItem(private val hero: Hero) : Item() {

    override fun bind(viewHolder: ViewHolder, position: Int) {
        viewHolder.itemView.txtHeroName.text = hero.name
        Picasso.get().load(hero.image).into(viewHolder.itemView.imgHeroes)
    }

    override fun getLayout(): Int = R.layout.item_hero
}
```

method **getLayout** ini adalah layout apa yang akan digunakan untuk tiap itemnya,
lalu method bind ini adalah dimana kalian dapat meng-set value ke tiap-tiap itemnya

## Set adapter ke Recyclerview

Selanjutnya adalah bagaimana menggunakan item ini ke recyclerview dan adapter yang sudah dideklarasi sebelumnya

```java
class MainActivity : AppCompatActivity(), HeroListener {

    // declare adapter from groupadapter
    private var groupAdapter = GroupAdapter<ViewHolder>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val listHeroes = listOf(
                Hero(name = "Spider-Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/9/30/538cd33e15ab7/standard_xlarge.jpg"),
                Hero(name = "Black Panther", image = "https://i.annihil.us/u/prod/marvel/i/mg/1/c0/537ba2bfd6bab/standard_xlarge.jpg"),
                Hero(name = "Iron Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/6/a0/55b6a25e654e6/standard_xlarge.jpg"),
                Hero(name = "Dead Pool", image = "https://i.annihil.us/u/prod/marvel/i/mg/5/c0/537ba730e05e0/standard_xlarge.jpg"),
                Hero(name = "Captain Marvel", image = "https://i.annihil.us/u/prod/marvel/i/mg/c/10/537ba5ff07aa4/standard_xlarge.jpg"),
                Hero(name = "Ant Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/6/90/54ad7297b0a59/standard_xlarge.jpg"),
                Hero(name = "Spider-Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/9/30/538cd33e15ab7/standard_xlarge.jpg"),
                Hero(name = "Black Panther", image = "https://i.annihil.us/u/prod/marvel/i/mg/1/c0/537ba2bfd6bab/standard_xlarge.jpg"),
                Hero(name = "Iron Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/6/a0/55b6a25e654e6/standard_xlarge.jpg"),
                Hero(name = "Dead Pool", image = "https://i.annihil.us/u/prod/marvel/i/mg/5/c0/537ba730e05e0/standard_xlarge.jpg"),
                Hero(name = "Captain Marvel", image = "https://i.annihil.us/u/prod/marvel/i/mg/c/10/537ba5ff07aa4/standard_xlarge.jpg"),
                Hero(name = "Ant Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/6/90/54ad7297b0a59/standard_xlarge.jpg")
        )

        // inisiasi recyclerview layout manager dan adapter
        rvMain.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = groupAdapter
        }

        // looping hero
        listHeroes.map {
        	// masukkan hero ke groupadater
            groupAdapter.add(HeroItem(it))
        }
    }

    override fun onHeroClick(hero: Hero) {
        Toast.makeText(this, "hero clicked ${hero.name}", Toast.LENGTH_SHORT).show()
    }
}

```

![Android RecyclerView](/assets/images/recyclerview/rv_7.png){:class="img-responsive"}

kode diatas adalah kode yang saya gunakan di artikel [sebelumnya](https://pratamawijaya.com/programming/android-recyclerview-kotlin/),
kuncinya untuk memasukkan tiap item ke recyclerview adalah pada kode
**groupAdapter.add(HeroItem(it))**

simple, cepat dan mudah untuk dicustom, misal kita memiliki multi view type, tinggal kita buatkan class item untuk view tersebut, contoh saya buat satu lagi class Item

```java
class HeroSecondItem(private val hero: Hero) : Item() {

    override fun bind(viewHolder: ViewHolder, position: Int) {
        viewHolder.itemView.txtHeroName.text = hero.name
        Picasso.get().load(hero.image).into(viewHolder.itemView.imgHeroes)
    }

    override fun getLayout(): Int = R.layout.item_hero_second
}
```

yang saya bedakan disini adalah layoutnya, untuk second item ini LinearLayout orientation saya ubah menjadi horizontal, hasilnya adalah

![Android RecyclerView](/assets/images/recyclerview/rv_6.png){:class="img-responsive"}

untuk item dengan nama awal "D" maka akan menggunakan item dengan layout orientation horizontal, 

kode pada tutorial ini dapat diunduh di [link-github](https://github.com/pratamawijaya/SimpleRecyclerviewKotlin/tree/groupie)