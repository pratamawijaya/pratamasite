---
layout: single
title:  "Tutorial Android : Membuat Image Slider Carousel Dengan RecyclerView"
date:   2018-07-26 12:00:00 +0700
excerpt: "Tutorial android membuat menu image slider carousel home seperti gojek/bukalapak/tokopedia dengan recyclerview"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
    - recyclerview
---

Pada artikel sebelumnya sudah saya bahas bagaimana membuat recyclerview dengan library groupie, dan pada artikel tersebut saya juga memberikan pernyataan bahwa groupie juga dapat mempermudah kita untuk membuat view yang memiliki banyak type atau case multiviewtype dengan recyclerview.

contohnya seperti apa ? misal halaman homepage aplikasi e-commerce yang ada di Indonesia, misal bukalapak

![Recyclerview MultiViewType](/assets/images/recyclerview/rv_bukalapak.png){:class="img-responsive"}

untuk bagian yang saya beri tanda nomor, itu kita akan pisah atau buatkan viewtypenya sendiri, misal untuk nomor 1 viewtypenya saya berinama **BannerCarouselItem** lalu untuk nomor 2 saya beri nama **ProductCategoryItem**, selanjutnya ? mari kita mulai coba buat

Untuk kode yang saya gunakan, saya masih menggunakan kode pada artikel sebelumnya, pertama kita buat dulu untuk view banner carouselnya, apabila diperhatikan carousel tersebut terdiri dari

- viewpager (gambar yang dapat berpindah)
- viewpagerindicator (yang berbentuk bulat2 kecil bagian kiri bawah)
- sebuat button kecil ada pada bagian kanan

## Persiapan layout

langkah pertama kita perlu membuat view untuk carousel banner kita, buat sebuah layout xml beri nama **item_carousel_banner.xml**

lalu isinya 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:attrs="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <android.support.v4.view.ViewPager
        android:id="@+id/viewPagerBanner"
        android:layout_width="match_parent"
        android:layout_height="@dimen/banner_height"
        android:focusableInTouchMode="false"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>

    <com.rd.PageIndicatorView
        android:id="@+id/indicator"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toBottomOf="@id/viewPagerBanner"
        app:piv_animationType="scale"
        app:piv_dynamicCount="true"
        app:piv_selectedColor="@color/colorAccent"
        app:piv_unselectedColor="@color/gray_700"
        attrs:piv_padding="12dp"
        attrs:piv_radius="8dp"/>

    <Button
        android:id="@+id/btnSemuaPromo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Semua Promo"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/viewPagerBanner"/>
</android.support.constraint.ConstraintLayout>
```

pada layout ini untuk indicator saya menggunakan library dari https://github.com/romandanylyk/PageIndicatorView, pastikan untuk terlebih dahulu menambahkan library tersebut ke file app/build.gradle

## Persiapan Model

untuk modelnya saya membuat seperti ini

```java
data class BannerPromo(val name: String,
                       val image: String)
```

## Item Layout

lalu untuk item layout groupie saya buat menjadi seperti ini 

```java
interface BannerListener {
    fun onSeeAllPromoClick()
    fun onBannerClick(promo: BannerPromo)
}

class BannerCarouselItem(private val banners: List<BannerPromo>,
                         private val supportFragmentManager: FragmentManager,
                         private val listener: BannerListener) : Item() {

    override fun bind(viewHolder: ViewHolder, position: Int) {
        val viewPagerAdapter = BannerAdapter(supportFragmentManager, banners)
        viewHolder.itemView.viewPagerBanner.adapter = viewPagerAdapter
        viewHolder.itemView.indicator.setViewPager(viewHolder.itemView.viewPagerBanner)

        viewHolder.itemView.btnSemuaPromo.setOnClickListener {
            listener.onSeeAllPromoClick()
        }
    }

    override fun getLayout(): Int = R.layout.item_carousel_banner
}
```

saya tambahkan interface untuk click listener button lihat semua promo, lalu di class inilah saya juga melakukan inisiasi untuk banner adapter, 

## Banner Adapter untuk viewpager

banner adapter disini digunakan untuk menampilkan fragment bannernya sendiri yang berisi gambar promo

```java
class BannerAdapter(fragmentManager: FragmentManager,
                    private val banners: List<BannerPromo>) : FragmentPagerAdapter(fragmentManager) {

    override fun getItem(pos: Int): Fragment {
        return BannerFragment.newInstance(banners[pos].image)
    }

    override fun getCount(): Int = banners.size

}
```

## Fragment untuk gambar banner

```java

class BannerFragment : Fragment() {

    companion object {
        /**
         * new instance pattern for fragment
         */
        @JvmStatic
        fun newInstance(url: String): BannerFragment {
            val newsFragment = BannerFragment()
            val args = Bundle()
            args.putString("img", url)
            newsFragment.arguments = args
            return newsFragment
        }
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
                              savedInstanceState: Bundle?): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_banner, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        val url = arguments?.getString("img")

        url.let {
            Picasso.get().load(url).into(imgBanner)
        }
    }


}

```

fungsin fragment ini adalah untuk menampilkan view/gambar promo banner yang akan kita passing melalui banner adapter, untuk layoutnya sendiri seperti ini

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="@dimen/banner_height"
                tools:context=".BannerFragment">

    <ImageView
        android:id="@+id/imgBanner"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:adjustViewBounds="true"
        android:scaleType="centerCrop"/>

</RelativeLayout>
```

## Gabungkan semua di activity

lalu selanjutnya tinggal menggabungkan bagian-bagian yang telah kita buat di class activity yang akan digunakan, kurang lebih masih sama dengan artikel saya sebelumnya tentang groupie library,

```java
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val promos = listOf(
                BannerPromo(name = "Puncak badai uang",
                        image = "https://s2.bukalapak.com/uploads/promo_partnerinfo_bloggy/2842/Bloggy_1_puncak.jpg"),
                BannerPromo(
                        name = "hati hati ada guncangan badai uang",
                        image = "https://s4.bukalapak.com/uploads/promo_partnerinfo_bloggy/5042/Bloggy_1.jpg"
                ),
                BannerPromo(name = "Puncak badai uang",
                        image = "https://s2.bukalapak.com/uploads/promo_partnerinfo_bloggy/2842/Bloggy_1_puncak.jpg"),
                BannerPromo(
                        name = "hati hati ada guncangan badai uang",
                        image = "https://s4.bukalapak.com/uploads/promo_partnerinfo_bloggy/5042/Bloggy_1.jpg"
                ),
                BannerPromo(name = "Puncak badai uang",
                        image = "https://s2.bukalapak.com/uploads/promo_partnerinfo_bloggy/2842/Bloggy_1_puncak.jpg"),
                BannerPromo(
                        name = "hati hati ada guncangan badai uang",
                        image = "https://s4.bukalapak.com/uploads/promo_partnerinfo_bloggy/5042/Bloggy_1.jpg"
                )
        )

        rvMain.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = groupAdapter
        }

        // declare banner carousel
        val bannerCarouselItem = BannerCarouselItem(promos, supportFragmentManager, this)


        groupAdapter.add(bannerCarouselItem)
    }
```

pertama inisiasi dulu data promo, selanjutnya tinggal kita inisiasi item untuk banner carousel, lalu terakhir set ke groupAdapter 

<video width="480" height="320" controls="controls">
  <source src="/assets/images/recyclerview/rv_bukalapak_video.mp4" type="video/mp4">
</video>