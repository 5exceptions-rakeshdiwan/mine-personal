# Applylar Android SDK Integration

Appylar for Android is a lightweight and easy-to-use Ad integration SDK provided by Appylar. SDK enables developers to integrate Appylar Ads in any type of application.

Appylar provides several type of ads and enables you to set ad where ever you want in application.

Provided Ads by Appylar such as:

 - Banners
 - Insterstitials
 
## General Requirements
 - SDK version must be Android 5.0 (API level 21) [Lollipop] and later.
 
# Usage
The installation guide will lead you through the following steps:
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add Appylar to Gradle</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Setup the configuration and listeners</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Setup the configuration</a>

# Step 1: Add Appylar to your Gradle
Set our jitpack artifact repository in your project's root build.gradle (or settings.gradle if you are using settings repositories):
```
[build.gradle (NameOfProject)]
...

allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}
```
Set dependency in your :app module's build.gradle:
```
[build.gradle (: app)]
...

dependencies {
	implementation 'com.github.appylar:android-sdk:#latest-version'
}
```

# Step 2: Setup the Configuration for your App and Listeners
1. Create arbitrary subclass of Application(), override it's onCreate() and implement interface of Events. You can, of course, use your Application subclass if you already have one in your project.
#### Kotlin
```
[AppylarApplication.kt]
import com.appylar.android.sdk.interfaces.Events

class AppylarApplication : Application(), Events {

    override fun onCreate() {
        super.onCreate()
	Appylar.addEventListener(this)
    }
    
    override fun onInitialized() {
         
    }

    override fun onError(error: String) {
        
    }
}
```
#### Java
```
[AppylarApplication.java]
import com.appylar.android.sdk.interfaces.Events;

public class AppylarApplication extends Application implements Events {
    @Override
    public void onCreate() {
        super.onCreate();
    }
    @Override
    public void onInitialized() {
                
    }

    @Override
    public void onError(@NonNull String error) {

    }
}
```
2. Add this new subclass to AndroidManifest.xml" inside <application> tag:
```
[AndroidManifest.xml]

<application
    android:name=".AppylarApplication"
    ...
```
3. How to add banner implementation.You need add banner component to your xml file.
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.appylar.android.sdk.bannerview.BannerView
        android:id="@+id/bannerViewTop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true" />

</RelativeLayout>
```	  
3. Find id of banner component by using findViewById to you Kotlin/Java file.
###### Kotlin
```
import com.appylar.android.sdk.bannerview.BannerView;

class KotlinActivity : AppCompatActivity() {
    var bannerView : BannerView? = null
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin)
        bannerView = findViewById(R.id.bannerView)
    }
}
```
###### Java
```
import com.appylar.android.sdk.bannerview.BannerView

public class JavaActivity extends AppCompatActivity {
    BannerView bannerView; 
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_java);
  	bannerView = findViewById(R.id.bannerView);
    }
}
```	

4. Setup the Configuration for your app. Set up with your values 😉
