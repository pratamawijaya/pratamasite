---
layout: single
title:  "Android Programming: Google Map - Hitung Jarak Antar 2 Titik"
date:   2018-06-18 16:30:21 +0700
excerpt: "Belajar menggunakan google maps dan menghitung jarak antar dua titik menggunakan Google Direction"
categories: 
    - Programming
tags : 
    - programming
    - android
    - google-maps
    - google-direction
---

Pada kesempatan kali ini saya membahas mengenai bagaimana cara menghitung jarak antar 2 titik dan menampilkannya kedalam google maps, aplikasi ini saya buat sesimple mungkin dengan menggunakan google maps untuk menampilkan mapsnya, 

Tahapan pertama adalah membuat activity google maps
cara membuat activity google maps pada android studio adalah dengan klik kanan pada nama package aplikasi anda, lalu pilih new > Google > Google Maps Activity

![Google Maps](/assets/images/google_maps/google_maps_1.png){:class="img-responsive"}

selanjutnya android studio akan otomatis setup library untuk google maps , selain itu class activity dan layoutnya juga sudah disiapkan, hal yang selanjutnya adalah setup api key untuk google mapsnya

![Google Maps](/assets/images/google_maps/google_maps_2.png){:class="img-responsive"}

selanjutnya untuk view utamanya saya buat simple seperti ini

![Google Maps](/assets/images/google_maps/google_maps_3.png){:class="img-responsive"}

terdiri dari beberapa tombol dan textview, dimana fungsinya tombol a ketika diklik akan membuka activity google maps yang sebelumnya sudah dibuat, tujuannya untuk mendapatkan latitude dan logitude dari dua tempat berbeda yang nantinya akan dihitung jaraknya, untuk berpindah ke activity google maps saya menggunakan startactivityfor result, agar hasil dari activity google maps dapat saya ambil kembali ke activity main saya, berikut kode di activity main

```java
    void onBtnAClick() {
        startActivityForResult(intentMaps, RC_BTN_A);
    }
```

lalu pada activity mapsnya untuk mendapatkan location yang dipilih saya hanya mengambil lokasi center layar user, kodenya seperti berikut ini

```java
    @OnClick(R.id.btnPilih)
    void onBtnPilihClick() {
        final LatLng currentLocation = mMap.getCameraPosition().target;
        Log.d(MapsActivity.class.getSimpleName(), "current location " + currentLocation.latitude);

        Intent returnIntent = new Intent();
        returnIntent.putExtra(MainActivity.KEY_LAT, currentLocation.latitude);
        returnIntent.putExtra(MainActivity.KEY_LNG, currentLocation.longitude);

        setResult(RESULT_OK, returnIntent);
        finish();
    }
```

selanjutnya result tersebut diproses di activity main pada method onActivityResult, dan saya tampilkan ke textview yang ada hanya sebagai tanda bahwa data berhasil diambil dari activity maps

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            final double lat = data.getDoubleExtra(KEY_LAT, 0);
            final double lng = data.getDoubleExtra(KEY_LNG, 0);

            switch (requestCode) {
                case RC_BTN_A:
                    tvLokasiA.setText("lat: " + lat + " lng: " + lng);
                    latStart = lat;
                    lngStart = lng;
                    break;
                case RC_BTN_B:
                    tvLokasiB.setText("lat: " + lat + " lng: " + lng);
                    latEnd = lat;
                    lngEnd = lng;
                    break;
            }
        }
    }
```

untuk proses perhitungan jarak saya membuat satu buah activity google maps kembali yang saya namakan DirectionActivity, lalu saya passing data ke activity tersebut dari activity main

```java
 @OnClick(R.id.btnHitung)
    void onBtnHitungClick() {
        Intent intent = new Intent(this, DirectionActivity.class);
        intent.putExtra(MainActivity.KEY_LAT_START, latStart);
        intent.putExtra(MainActivity.KEY_LNG_START, lngStart);
        intent.putExtra(MainActivity.KEY_LAT_END, latEnd);
        intent.putExtra(MainActivity.KEY_LNG_END, lngEnd);
        startActivity(intent);
    }

```

lalu untuk menghitung jarak saya menggunakan library google direction api yang saya ambil dari https://github.com/jd-alexander/Google-Directions-Android

kode penggunaan library direction

```java
     @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_direction);
        ButterKnife.bind(this);

        latStart = getIntent().getDoubleExtra(MainActivity.KEY_LAT_START, 0);
        lngStart = getIntent().getDoubleExtra(MainActivity.KEY_LNG_START, 0);
        latEnd = getIntent().getDoubleExtra(MainActivity.KEY_LAT_END, 0);
        lngEnd = getIntent().getDoubleExtra(MainActivity.KEY_LNG_END, 0);

        LatLng start = new LatLng(latStart, lngStart);
        LatLng end = new LatLng(latEnd, lngEnd);

        Routing routing = new Routing.Builder()
                .travelMode(AbstractRouting.TravelMode.DRIVING)
                .key("key-direction-api")
                .waypoints(start, end)
                .withListener(this)
                .build();
        routing.execute();

```

pada library tersebut kita perlu implement listener, sebagaimana pada config sebelumnya kita set listener dengan method withListener(this), method utama yang dibutuhkan untuk menggambar directionnya adalah method onRoutingSucces, kodenya adalah sebagai berikut,

```java
 @Override
    public void onRoutingSuccess(ArrayList<Route> routes, int i) {
        Log.d("tag", "routing success " + routes.size());

        mMap.addMarker(new MarkerOptions().position(new LatLng(latStart, lngStart)));
        mMap.addMarker(new MarkerOptions().position(new LatLng(latEnd, lngEnd)));
        mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(latStart, lngStart), 14));

        String txtInfo = "";

        for (Route data : routes) {
            // draw polyline
            Log.d("tag", "write polyline " + data.getDistanceText());
            txtInfo += data.getDistanceText() + " " + data.getDurationText();

            PolylineOptions polylineOptions = new PolylineOptions();
            polylineOptions.width(10);
            polylineOptions.color(Color.RED);
            polylineOptions.addAll(data.getPoints());
            mMap.addPolyline(polylineOptions);
        }

        tvInfo.setText(txtInfo);

    }

```

kode lengkap sample projek diatas dapat diunduh di url berikut https://github.com/pratamawijaya/google-maps-direction-sample