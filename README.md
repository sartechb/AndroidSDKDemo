#AdAdapted Android SDK Quickstart

The following guide will help you set up the AdAdapted Android SDK in **Android Studio**. These directions are untested for **eclipse** as of now. Please check back for regular updates!

###Install
It's really simple to install the Android SDK. 

Download the AdAdapted Android SDK [here]().

If you don't have an existing **Android Studio** project create a new one. Drag the SDK file into your project libs directory. Here's an example file path that shows you where the libs directory is loaded. Here, RecipesV2 is the name of the project.

//Todo: fix image
![filepath](quickstart_assets/filepath.png)

Under your gradle scripts we need to modify both build.gradle files. The first build.gradle file we'll modify is the **project** one. In the repositories, under all projects, paste the following:

	flatDir{
		dirs 'libs'
	}
	
The final codeblock should look something like

	allprojects{
		repositories{
			jcenter()
			flatDir{
				dirs 'libs'
			}
		}
	}
	
In the second build.gradle file **(module:app)** add the following two lines in your dependencies:

```
compile(name: 'sdk-release', ext: 'aar')
compile 'com.google.android.gms:play-services:7.5.0'
```
*note: play-services may have been updated since the last revision of this Quickstart guide. You should still be able to use the latest version of play services without any troubles.*

The last step is to do a gradle sync. After this you should be good to go!

###Initialize

There's only a few steps we need to take to initialize the AdAdapted Android SDK. The first thing we need to do is to create our own Application class. If you don't have one already, create a new Java class and have it extend `android.app.Application`. Inside the class definition paste the following

	@Override
	public void onCreate(){
		super.onCreate();
		
		/*
		Replace these strings with your zone number(s)
		All numbers must be in string form.
		For example, if you had a zone with an id of 000000
		inside this array you'd put "000000"
		*/
		String[] zones = {“zone1”, “zone2”, “zone3”};
		
		//Rplace the API_KEY with the key provided to you
		//If your app is in production, change the false to true.
		AdAdapted.init(this, “API_KEY”, zones, false);
	}
	
Add the following permissions to your manifest file:

```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```
Also add the follwing activity inside the `<application>` tag:

```
<activity
 android:name="com.adadapted.android.sdk.ui.activity.AaWebViewPopupActivity"
 android:label="@string/title_activity_web_view_popup">
</activity>
```
That's it! In the next section we'll test the SDK to make sure that our ads display.
###Test
Inside one of your visible layout files add the following View:

```
<com.adadapted.android.sdk.ui.view.AaZoneView
    android:id="@+id/mainActivityAd"
    android:layout_width="match_parent"
    android:layout_height="50dp"/>
```
In this example we put the View in `activity_main.xml` and gave the ad the id mainActivityAd. In the corresponding Java file (in our example `MainActivity.java`), inside your `onCreate()` method link to the ad in the View and initialize it with a zone, like so:

```
AaZoneView mainActivtyAd = findViewById(R.id.mainActivityAd);
mainActivityAd.init("zone_id")
```
Your zone_id would be one of the zones you used when you initialized all your zones in your `Application#onCreate()` method.

One last thing to wrap up, inside your Activity you need to add the following two methods:

```
@Override
public void onStart() {
    super.onStart();
    mainActivityAd.onStart();
}
 
@Override
public void onStop() {
    super.onPause();
    mainActivityAd.onStop();
}

```

That's it! Compile and run your app and you should see your ad! Welcome to the AdAdapted family, we can't wait to see how you incorporate native ads.
