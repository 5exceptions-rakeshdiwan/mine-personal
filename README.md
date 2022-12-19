# Applylar Android SDK Integration

Appylar library for Android is a lightweight and easy-to-use Ad integration SDK provided by Appylar. SDK enables developers to integrate Appylar Ads in any type of application.

Appylar provides several types of Ads and enables you to set Ads wherever you want in the application.

The Ads provided by Appylar are like:

 - Banners
 - Insterstitials
 
## General Requirements
 - Targeted versions must be Android 5.0 (API level 21) [Lollipop] and later.
 
# Usage
The installation guide will lead you through the following steps:
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add Appylar to Gradle</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Setup the SDK configuration</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add BannerView to the application</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add Interstitial to the application</a>

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

# Step 2: Setup the configuration for your App and Listeners
1. Create arbitrary subclass of Application(), override it's onCreate() method and implement `Events` interface. You can, of course, use your Application subclass, if you already have one in your project.
#### Kotlin
```
[AppylarApplication.kt]
import com.appylar.android.sdk.Appylar
import com.appylar.android.sdk.interfaces.Events

class AppylarApplication : Application(), Events {

    override fun onCreate() {
        super.onCreate()
	Appylar.addEventListener(this) //attach callback listeners for SDK before initialization
	//Initialization
	...
    }
    
    override fun onInitialized() {
         //Callback for successful initialization
    }

    override fun onError(error: String) {
        //Callback for error thrown by SDK
    }
}
```
#### Java
```
[AppylarApplication.java]
import com.appylar.android.sdk.Appylar;
import com.appylar.android.sdk.interfaces.Events;

public class AppylarApplication extends Application implements Events {
    @Override
    public void onCreate() {
        super.onCreate();
	Appylar.addEventListener(this); //Attach callback listeners for SDK before initialization
	//Initialization
	...
    }
    @Override
    public void onInitialized() {
	//Callback for successful initialization
    }

    @Override
    public void onError(@NonNull String error) {
	//Callback for error thrown by SDK
    }
}
```
2. Initialize SDK with configuration:
#### Kotlin
```
[AppylarApplication.kt]
override fun onCreate() {
	super.onCreate()
	Appylar.addEventListener(this) //Attach callback listeners for SDK before initialization
	//Initialization
	 Appylar.init(
                this, //Application context 
                "<YOUR_APP_KEY>", 	//APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"] 
                arrayOf(AdType.BANNER, AdType.INTERSTITIAL), 	//What type of Ads you want to integrate 
                arrayOf(Orientation.PORTRAIT, Orientation.LANDSCAPE), 	//Supported orientations for Ads
                true 	//Test Mode, [TRUE] for development & [FALSE] for production
            )
}
```
#### Java
```
@Override
public void onCreate() {
	super.onCreate();
	Appylar.addEventListener(this);//Attach callback listeners for SDK before initialization
	//Initialization
	Appylar.init(
		this, //application context 
		"<YOUR_APP_KEY>", 	//APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"]
		new AdType[] {AdType.BANNER, AdType.INTERSTITIAL}, 	//What type Ads you want to integrate 
		new Orientation[] {Orientation.PORTRAIT, Orientation.LANDSCAPE}, 	//Supported orientations for Ads
                true 	//Test Mode, [TRUE] for development & [FALSE] for production
	); 
}
```
3. Add this new subclass to AndroidManifest.xml" inside <application> tag:
```
[AndroidManifest.xml]

<application
    android:name=".AppylarApplication"
    ...
```
# Step 3: Add BannerView to the application
1. To integrate the BannerView component in your design, prefer below snippet:
```
<com.appylar.android.sdk.bannerview.BannerView
	android:id="@+id/bannerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
```	  
2. Bind component to your activity and implement callback listeners.
###### Kotlin
```
import com.appylar.android.sdk.bannerview.BannerView

class KotlinActivity : AppCompatActivity() : BannerViewListener {
    	private var bannerView : BannerView? = null
    
    	override fun onCreate(savedInstanceState: Bundle?) {
        	super.onCreate(savedInstanceState)
        	setContentView(R.layout.activity_kotlin)
        	bannerView = findViewById(R.id.bannerView)
		bannerView.addEventListener(this) //Attach Event Listeners
    	}
	
	override fun onNoAd() {
        	//Callback if there is not Ad available to show
	}

	override fun onAdShown() {
		//Callback for Ad show success
	}
}
```
###### Java
```
import com.appylar.android.sdk.bannerview.BannerView;

public class JavaActivity extends AppCompatActivity implements BannerViewListener {
	private BannerView bannerView; 
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	super.onCreate(savedInstanceState);
        	setContentView(R.layout.activity_java);
  		bannerView = findViewById(R.id.bannerView);
		bannerView.addEventListener(this); //Attach Event Listeners
    	}
	
	@Override
        public void onNoAd() {
		//Callback if there is not Ad available to show
	}
	
	@Override
        public void onAdShown() {
		//Callback for Ad show success
	}
}
```	
3. Check Ad availablity and show the Ad.
###### Kotlin
```
bannerView.addEventListener(this) //Attach Event Listeners
if(canShowAd()) {
	//BannerView to show Ad on the UI with optional customized placement
	//Leave blank if not required
	bannerView.ShowAd("<PLACEMENT_STRING>") 	
}

```
###### Java
```
bannerView.addEventListener(this); //Attach Event Listeners
if(canShowAd()) { //Check Ad availability
	//BannerView to show Ad on the UI with optional customized placement
	//Leave blank if not required
	bannerView.ShowAd("<PLACEMENT_STRING>");	
}
```

# Step 4: Add Interstitial to the application
1. Implement callback listeners for Interstitial.
###### Kotlin
```
import com.appylar.android.sdk.interstitialviews.Interstitial

class KotlinActivity : AppCompatActivity() : InterstitialListener {
    	override fun onCreate(savedInstanceState: Bundle?) {
        	super.onCreate(savedInstanceState)
        	setContentView(R.layout.activity_kotlin)
	
		Interstitial.addEventListener(this) //Attach Event Listeners
    	}
	
	override fun onNoAd() {
        	//Callback if there is not Ad available to show
	}

	override fun onAdShown() {
		//Callback for Ad show success
	}
	
	override fun onInterstitialClosed() {
		//Callback for close event of interstitial
	}
}
```
###### Java
```
import com.appylar.android.sdk.interstitialviews.Interstitial;

public class JavaActivity extends AppCompatActivity implements InterstitialListener {
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	super.onCreate(savedInstanceState);
        	setContentView(R.layout.activity_java);
	
  		Interstitial.addEventListener(this); //Attach Event Listeners
    	}
	
	@Override
        public void onNoAd() {
		//Callback if there is not Ad available to show
	}
	
	@Override
        public void onAdShown() {
		//Callback for Ad show success
	}
	
	@Override
        public void onInterstitialClosed() {
		//Callback for close event of interstitial
	}
}
```	
3. Check Ad availablity and show the Ad.
###### Kotlin
```
Interstitial.addEventListener(this) //Attach Event Listeners
if(Interstitial.canShowAd()) {
	//Interstitial to show Ad on the UI with context of activity and optional customized placement
	//Leave blank if not required
	Interstitial.ShowAd(this, "<PLACEMENT_STRING>")
}
```
###### Java
```
Interstitial.addEventListener(this); //Attach Event Listeners
if(Interstitial.canShowAd()) { //Check Ad availability
	//Interstitial to show Ad on the UI with context of activity and optional customized placement
	//Leave blank if not required
	bannerView.ShowAd("<PLACEMENT_STRING>");	
}
```
