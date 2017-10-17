
# Retrofit

* String response

```
    @GET("/pet/adore")
    Observable<String> getString();

   ...
   
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl(mBaseUrl)
            .addConverterFactory(ScalarsConverterFactory.create())    // for String
            .addConverterFactory(GsonConverterFactory.create(gson))
            .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
            .client(okHttpClient)
                .build();
```
