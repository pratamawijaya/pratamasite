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

pertama inisiasi dulu data promo, selanjutnya tinggal kita inisiasi item untuk banner carousel, lalu terakhir set ke groupAdapter , hasil akhir bisa dilihat di video berikut

<video width="480" height="320" controls="controls">
  <source src="/assets/images/recyclerview/rv_bukalapak_video.mp4" type="video/mp4">
</video>

selanjutnya adalah membuat item berbentuk grid untuk menampilkan produk-produk yang dijual, pertama buat dulu layout untuk grid category product ini, buat file xml **item_product_category.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="16dp"
        android:fontFamily="@font/roboto"
        android:text="Product category"
        android:textColor="@color/textPrimary"/>

    <android.support.v7.widget.RecyclerView
        android:id="@+id/rvProductCategory"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp">

    </android.support.v7.widget.RecyclerView>
</LinearLayout>

```

## Class item ProductCategoryItem

```java
class ProductCategoryItem(private val groupAdapter: GroupAdapter<ViewHolder>) : Item() {

    override fun bind(viewHolder: ViewHolder, position: Int) {
        val dimenDp = viewHolder.itemView.context.resources.getDimensionPixelSize(R.dimen._2sdp)
        val column = 3

        viewHolder.itemView.rvProductCategory.apply {
            adapter = groupAdapter
            layoutManager = GridLayoutManager(viewHolder.itemView.context, column)
            addItemDecoration(GridItemDecoration(dimenDp, column))
        }
    }

    override fun getLayout(): Int = R.layout.item_product_category
}
```

class ini untuk handle gridlayout product categorynya, disini saya menggunakan recyclerview untuk menampilkan secara grid, dan juga saya menambahkan itemdecoration agar sedikit rapi

```java
class GridItemDecoration(private val mSizeGridSpacingPx: Int, private val mGridSize: Int) : RecyclerView.ItemDecoration() {

    private var mNeedLeftSpacing = false

    override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {
        val frameWidth = ((parent.width - mSizeGridSpacingPx.toFloat() * (mGridSize - 1)) / mGridSize).toInt()
        val padding = parent.width / mGridSize - frameWidth
        val itemPosition = (view.layoutParams as RecyclerView.LayoutParams).viewAdapterPosition
        if (itemPosition < mGridSize) {
            outRect.top = 0
        } else {
            outRect.top = mSizeGridSpacingPx
        }
        if (itemPosition % mGridSize == 0) {
            outRect.left = 0
            outRect.right = padding
            mNeedLeftSpacing = true
        } else if ((itemPosition + 1) % mGridSize == 0) {
            mNeedLeftSpacing = false
            outRect.right = 0
            outRect.left = padding
        } else if (mNeedLeftSpacing) {
            mNeedLeftSpacing = false
            outRect.left = mSizeGridSpacingPx - padding
            if ((itemPosition + 2) % mGridSize == 0) {
                outRect.right = mSizeGridSpacingPx - padding
            } else {
                outRect.right = mSizeGridSpacingPx / 2
            }
        } else if ((itemPosition + 2) % mGridSize == 0) {
            mNeedLeftSpacing = false
            outRect.left = mSizeGridSpacingPx / 2
            outRect.right = mSizeGridSpacingPx - padding
        } else {
            mNeedLeftSpacing = false
            outRect.left = mSizeGridSpacingPx / 2
            outRect.right = mSizeGridSpacingPx / 2
        }
        outRect.bottom = 0
    }
}
```

## Layout untuk tiap product item

selanjutnya perlu juga untuk membuat layout untuk tiap icon productnya, saya membuat **item_product.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:orientation="vertical"
              android:padding="16dp">

    <ImageView
        android:id="@+id/productImage"
        android:layout_width="80dp"
        android:layout_height="80dp"/>

    <TextView
        android:id="@+id/productName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="16dp"
        android:fontFamily="@font/roboto"
        android:text="Badai Uang"
        android:textColor="@color/textPrimary"/>

</LinearLayout>
```

lalu untuk class itemnya sebagai berikut

```java
interface ProductListener {
    fun onProductClicked(product: Product)
}

class ProductItem(private val product: Product,
                  private val listener: ProductListener) : Item() {

    override fun bind(viewHolder: ViewHolder, position: Int) {
        viewHolder.itemView.productName.text = product.name
        Picasso.get().load(product.img).into(viewHolder.itemView.productImage)

        viewHolder.itemView.setOnClickListener {
            listener.onProductClicked(product)
        }
    }

    override fun getLayout(): Int = R.layout.item_product

}
```

lalu langkah terakhir ubah class activity menjadi seperti berikut

```java
class MainActivity : AppCompatActivity(), HeroListener, BannerListener, ProductListener {

    // declare adapter from groupadapter
    private var groupAdapter = GroupAdapter<ViewHolder>()

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

        val products = listOf(
                Product(name = "Steam",
                        img = "http://www.stickpng.com/assets/images/58aefdc2c869e092af51ee6f.png"),
                Product(name = "Starbucks",
                        img = "https://news.starbucks.com/uploads/images/Logo/_preview/Starbucks_Logo_Hi-res.jpg"),
                Product(name = "Steam",
                        img = "http://www.stickpng.com/assets/images/58aefdc2c869e092af51ee6f.png"),
                Product(name = "Starbucks",
                        img = "https://news.starbucks.com/uploads/images/Logo/_preview/Starbucks_Logo_Hi-res.jpg"),
                Product(name = "Steam",
                        img = "http://www.stickpng.com/assets/images/58aefdc2c869e092af51ee6f.png"),
                Product(name = "Starbucks",
                        img = "https://news.starbucks.com/uploads/images/Logo/_preview/Starbucks_Logo_Hi-res.jpg"),
                Product(name = "Steam",
                        img = "http://www.stickpng.com/assets/images/58aefdc2c869e092af51ee6f.png"),
                Product(name = "Starbucks",
                        img = "https://news.starbucks.com/uploads/images/Logo/_preview/Starbucks_Logo_Hi-res.jpg"),
                Product(name = "Steam",
                        img = "http://www.stickpng.com/assets/images/58aefdc2c869e092af51ee6f.png"),
                Product(name = "Starbucks",
                        img = "https://news.starbucks.com/uploads/images/Logo/_preview/Starbucks_Logo_Hi-res.jpg")
        )



        rvMain.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = groupAdapter
        }

        // declare banner carousel
        val bannerCarouselItem = BannerCarouselItem(promos, supportFragmentManager, this)
        groupAdapter.add(bannerCarouselItem)

        Section().apply {
            add(makeProductCategory(products))
            groupAdapter.add(this)
        }
    }

    private fun makeProductCategory(products: List<Product>): ProductCategoryItem {
        val productItemGroupAdapter = GroupAdapter<ViewHolder>()
        products.map {
            productItemGroupAdapter.add(ProductItem(it, this))
        }
        return ProductCategoryItem(productItemGroupAdapter)
    }

    override fun onProductClicked(product: Product) {
        Toast.makeText(this, "clicked ${product.name}", Toast.LENGTH_SHORT).show()
    }

    override fun onSeeAllPromoClick() {
        Toast.makeText(this, "see all promo", Toast.LENGTH_SHORT).show()
    }

    override fun onBannerClick(promo: BannerPromo) {
    }

    override fun onHeroClick(hero: Hero) {
        Toast.makeText(this, "hero clicked ${hero.name}", Toast.LENGTH_SHORT).show()
    }
}

```

hasil akhirnya akan seperti berikut ini

<video width="480" height="320" controls="controls">
  <source src="/assets/images/recyclerview/rv_bukalapak_video_2.mp4" type="video/mp4">
</video>

untuk source code code diatas bisa diakses ke https://github.com/pratamawijaya/SimpleRecyclerviewKotlin/tree/homepage

jangan lupa share, dan semoga bermanfaat