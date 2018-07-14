---
layout: single
title:  "Tutorial Android : Cara menggunakan retrofit dengan bahasa kotlin"
date:   2018-07-11 15:00:00 +0700
excerpt: "Tutorial dasar android, setup retrofit dengan bahasa kotlin di android programming"
categories: 
    - Programming
tags : 
    - tutorial-android
    - kotlin
    - android
    - retrofit
    - rest
    - api
---

Pada kesempatan kali ini saya akan membahas **tutorial android** bagaimana caranya menggunakan retrofit sebagai http client dengan menggunakan kotlin sebagai bahasa pemrograman di android, pertama yang harus dilakukan adalah menambahkan dependency retrofit pada project android anda, tambahkan code berikut pada file app/build.gradle

```
 implementation "com.squareup.retrofit2:retrofit:2.4.0"
 implementation "com.squareup.retrofit2:converter-gson:2.4.0"
```

lalu untuk tutorial ini saya akan menggunakan **https://jsonplaceholder.typicode.com/posts** sebagai data dummynya, dengan bentuk object jsonnya sebagai berikut

```json
[
	{
		"userId": 1,
		"id": 1,
		"title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
		"body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
	}
]
```

dari bentuk structure  json tersebut selanjutnya adalah membuat class pojo mengikuti structure json tersebut, berikut contoh class pojonya

```java
data class PostModel(val userId:Int,
                     val id:Int,
                     val title:String,
                     val body:String)
```

saya beri nama **PostModel** 

selanjutnya adalah membuat interface untuk servicesnya, saya membuat seperti ini

```java
interface PostServices {

    @GET("posts")
    fun getPosts(): Call<List<PostModel>>

}
```

dimana **GET("posts")** adalah endpoint untuk webservicesnya, lalu tahap selanjutnya adalah kita buat class repository untuk jembatan mengakses data ke services, class ini saya buat dengan object class pada kotlin, dimana class ini ketika di compile ke java maka akan menjadi final class

```java
object DataRepository {

    fun create(): PostServices {
        val retrofit = Retrofit.Builder()
                .addConverterFactory(GsonConverterFactory.create())
                .baseUrl("https://jsonplaceholder.typicode.com/")
                .build()
        return retrofit.create(PostServices::class.java)
    }
}
```

lalu untuk memanggilnya di activity akan seperti ini,

```java
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)


        // get post data
        val postServices = DataRepository.create()
        postServices.getPosts().enqueue(object : Callback<List<PostModel>> {

            override fun onResponse(call: Call<List<PostModel>>, response: Response<List<PostModel>>) {
                if (response.isSuccessful) {
                    val data = response.body()
                    Log.d("tag", "responsennya ${data?.size}")

                    data?.map {
                        Log.d("tag", "datanya ${it.body}")
                    }
                }
            }

            override fun onFailure(call: Call<List<PostModel>>, error: Throwable) {
                Log.e("tag", "errornya ${error.message}")
            }
        })
    }
}
```

cukup mudah dan simple penggunaan Retrofit dengan bahasa kotlin pada pemrograman android ini, 
selamat mencoba rekan - rekan pembaca yang budiman.
