---
layout: single
title:  "Tutorial Android : Membuat Recyclerview dengan kotlin"
date:   2018-07-24 08:00:00 +0700
excerpt: "Membuat list dengan recyclerview dan menggunakan bahasa kotlin"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
    - recyclerview
---

Recyclerview merupakan komponent dasar di android untuk menampilkan data berupa list, dimana kita tidak mengetahui berapa banyak data yang akan ditampilkan, jadi recyclerview memungkinkan kita untuk menambah data secara dynamic kedalam view kita.

Recyclerview memiliki 3 komponen utama :

- Layout
- ViewHolder
- Adapter

**Layout** adalah view yang akan dibuat untuk setiap item yang akan ditampilkan kedalam recyclerview

**ViewHolder** digunakan untuk cache view object kedalam memory

**Adapter** digunakan untuk create setiap item lalu dimasukkan ke viewholdernya dengan data yang kita berikan.

langkah pertama untuk menggunakan recyclerview adalah dengan menambahkan dependency recyclerview, tambahkan kode dibawah ke app/build.gradle

```
implementation ‘com.android.support:recyclerview-v7:x.x.x’
```

dimana **x.x.x** ini adalah versi support library yang akan digunakan, untuk layout activity_main saya buat seperti ini 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/rvMain"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</RelativeLayout>
```

kemudian kita selanjutnya butuh membuat layout untuk setiap item recyclerviewnya, saya buat layout dengan nama item_hero.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:orientation="vertical"
              android:padding="8dp">

    <TextView
        android:id="@+id/txtHeroName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

item layout disini masih saya buat simple, dimana hanya menampilkan nama hero saja.

lalu selanjutnya buat Pojo class untuk menampung data hero,

```java
data class Hero(
        val name: String,
        val image: String
)
```

selanjutnya kita siapkan adapternya terlebih dahulu, pertama buat class holdernya terlebih dahulu

```java
class HeroHolder(view: View) : RecyclerView.ViewHolder(view) {
    private val tvHeroName = view.txtHeroName

    fun bindHero(hero: Hero) {
        tvHeroName.text = hero.name
    }
}
```

selanjutnya class adapter

```java
class HeroAdapter(private val heroes: List<Hero>) : RecyclerView.Adapter<HeroHolder>() {

    override fun onCreateViewHolder(viewGroup: ViewGroup, p1: Int): HeroHolder {
        return HeroHolder(LayoutInflater.from(viewGroup.context).inflate(R.layout.item_hero, viewGroup, false))
    }

    override fun getItemCount(): Int = heroes.size

    override fun onBindViewHolder(holder: HeroHolder, position: Int) {
        holder.bindHero(heroes[position])
    }
}
```

ketika digabungn akan menjadi seperti ini

```java
class HeroAdapter(private val heroes: List<Hero>) : RecyclerView.Adapter<HeroHolder>() {

    override fun onCreateViewHolder(viewGroup: ViewGroup, p1: Int): HeroHolder {
        return HeroHolder(LayoutInflater.from(viewGroup.context).inflate(R.layout.item_hero, viewGroup, false))
    }

    override fun getItemCount(): Int = heroes.size

    override fun onBindViewHolder(holder: HeroHolder, position: Int) {
        holder.bindHero(heroes[position])
    }
}

class HeroHolder(view: View) : RecyclerView.ViewHolder(view) {
    private val tvHeroName = view.txtHeroName

    fun bindHero(hero: Hero) {
        tvHeroName.text = hero.name
    }
}
```

selanjutnya tinggal digunakan ke MainActivity,

```java
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val listHeroes = listOf(
                Hero(name = "Thor", image = ""),
                Hero(name = "Captain America", image = ""),
                Hero(name = "Iron Man", image = "")
        )

        val heroesAdapter = HeroAdapter(listHeroes)

        rvMain.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = heroesAdapter
        }
    }
}

```

hasil akhirnya ketika dirunning akan seperti ini

![Android RecyclerView](/assets/images/recyclerview/rv_1.png){:class="img-responsive"}

lalu misalkan kita mau menampilkan gambar di tiap item hero, kita bisa mengubah item layoutnya menjadi seperti ini

**item_hero.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:orientation="horizontal"
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
        android:layout_gravity="center_vertical"
        android:layout_marginLeft="16dp"/>

</LinearLayout>
```

lalu tambahkan url gambar untuk tiap hero,

```java
val listHeroes = listOf(
                Hero(name = "Spider-Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/9/30/538cd33e15ab7/standard_xlarge.jpg"),
                Hero(name = "Black Panther", image = "https://i.annihil.us/u/prod/marvel/i/mg/1/c0/537ba2bfd6bab/standard_xlarge.jpg"),
                Hero(name = "Iron Man", image = "https://i.annihil.us/u/prod/marvel/i/mg/6/a0/55b6a25e654e6/standard_xlarge.jpg")
        )
```

agar dapat menampilkan gambar via url, butuh library tambahan yakni **Picasso**
tambahkan dependency picasso ke app/build.gradle

```
implementation 'com.squareup.picasso:picasso:2.71828'
```

jangan lupa tambah permission internet ke AndroidManifest.xml

selanjutnya picasso akan digunakan di class HeroHolder, ubah menjadi seperti ini

```java
class HeroHolder(view: View) : RecyclerView.ViewHolder(view) {
    private val tvHeroName = view.txtHeroName
    private val imgHero = view.imgHeroes

    fun bindHero(hero: Hero) {
        tvHeroName.text = hero.name
        Picasso.get().load(hero.image).into(imgHero)
    }
}
```

maka hasil akhirnya akan menjadi seperti ini

![Android RecyclerView](/assets/images/recyclerview/rv_2.png){:class="img-responsive"}

Sekian tutorial singkat bagaimana cara menggunakan recyclerview dengan meggunakan kotlin sebagai bahasa pemrograman, semoga bermanfaat.


