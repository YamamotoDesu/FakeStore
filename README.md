# FakeStore
e-commerce application using the Redux (MVI) pattern

## E-commerce App Intro & Retrofit Setup | Redux (MVI) | Android 2022 | Kotlin
https://www.youtube.com/watch?v=wdRsci0u5lo&list=PLLgF5xrxeQQ2qeszlLJTuL9ZO4bSpngQr&index=1

build.gradle
```gradle
dependencies {

    // Retrofit
    def retrofit_version = "2.9.0"
    implementation "com.squareup.retrofit2:retrofit:$retrofit_version"
    implementation "com.squareup.retrofit2:converter-moshi:$retrofit_version"

    def lifecycle_version = "2.5.1"
    // Lifecycles only (without ViewModel or LiveData)
    implementation "androidx.lifecycle:lifecycle-runtime-ktx:$lifecycle_version"
```

MainActivity.kt
```kt
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val retrofit = Retrofit.Builder()
            .baseUrl("https://fakestoreapi.com/")
            .addConverterFactory(MoshiConverterFactory.create())
            .build()

        val productsService = retrofit.create(ProductsService::class.java)

        lifecycleScope.launchWhenCreated {
            val response = productsService.getAllProducts()
            Log.i("DATA", response.body()!!.toString())
        }
    }

    interface ProductsService {
        @GET("products")
        suspend fun getAllProducts(): Response<List<Any>>
    }
}
```
