
* Project build.gradle

```
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.3'
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
```

* Module build.gradle

```
    // Dagger
    apt 'com.google.dagger:dagger-compiler:2.5'
    compile 'com.google.dagger:dagger:2.5'
    provided 'javax.annotation:jsr250-api:1.0'


```
