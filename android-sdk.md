
###<a name="index">Index</a>
* [SDK download and setup](#install)
* [SDK API reference](#api)
* [SDK error code table](#errorcode)
* [Sample code](#sample)
* [How to apply Facebook/Admob advertisement](#reference)

###<a name="install">SDK download and setup</a>

* [Download the SDK](http://git.oschina.net/CloudTech/Android_SDK/attach_files)
* Build tool：Gradle
* After download, copy file adlibrary.jar into target project folder: [ModuleName]/libs/adlibrary.jar
* Update the project's build.gradle with below code:

```
    //buildscript::dependencies
    classpath 'com.google.gms:google-services:3.0.0'  
``` 

* Update the module's build.gradle with dependency：

```
apply plugin: 'com.google.gms.google-services'

dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.google.firebase:firebase-ads:9.0.0'
    compile 'com.facebook.android:audience-network-sdk:4.13.0'
    compile files('libs/adlibrary.jar')
}
```

* Update AndroidManifest.xml as below:

```
        <activity android:name="com.cloudtech.ads.view.InnerWebLandingActivity"
            android:launchMode="singleInstance">
            <intent-filter>
                <action android:name="com.cloudtech.action.InnerWebLanding" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>

        <activity android:name="com.cloudtech.ads.view.InterstitialActivity"
            android:launchMode="singleInstance">
        </activity>

        <activity android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
            android:theme="@android:style/Theme.Translucent" />
        <receiver android:name="com.cloudtech.ads.broadcast.LogBroadcast">
            <intent-filter>
                <action android:name="com.cloudtech.ads.SWITCH"/>
            </intent-filter>
        </receiver>
```



###<a name="api">SDK API reference</a>：

### CTService: The calling interface for the SDK.

```
 
    /**
     * Get banner style advertisement.
     * @param slotId banner advertisement space id
     * @param fbId facebook placement id.
     * @param adMobAppId admob application id
     * @param adMobUnitId admit banner id
     * @param isShowCloseButton show close button at the top-right corner of the advertisement
     * @param context Activity or application context.
     * @param isTest Use test advertisement or not
     * @param adListener Callback for the advertisement load process.
     * @return An container view which include advertisement.
     */
    public static CTNative getBanner(String slotId,
                                      String fbId,
                                      String adMobAppId,
                                      String adMobUnitId,
                                      boolean isShowCloseButton,
                                      Context context,
                                      Boolean isTest,
                                      CTAdEventListener adListener) 

    /**
     * Preload interstitial advertisement
     * @param slotId interstitial advertisement space id
     * @param fbId facebook placement id.
     * @param adMobAppId admob application id
     * @param adMobUnitId admit interstitial id
     * @param isShowCloseButton show close button at the top-right corner of the advertisement
     * @param context Activity or application context.
     * @param isTest Use test advertisement or not
     * @param adListener Callback for the advertisement load process.
     * @return An container view which include advertisement.
     */
    public static CTNative preloadInterstitial(String slotId,
                                               String fbId,
                                               String adMobAppId,
                                               String adMobUnitId,
                                               boolean isShowCloseButton,
                                               Context context,
                                               Boolean isTest,
                                               CTAdEventListener adListener)


    /**
     * Get native advertisement
     * @param slotId natvie advertisement space id
     * @param fbId facebook placement id.
     * @param adMobAppId admob application id
     * @param adMobUnitId admit native id (Not supported at present, pass null is ok)
     * @param isShowCloseButton show close button at the top-right corner of the advertisement
     * @param context Activity or application context.
     * @param isTest Use test advertisement or not
     * @param adListener Callback for the advertisement load process.
     * @return An container view which include advertisement.
     */
    public static CTNative getNative(String slotId,
                                     String fbId,
                                     String adMobAppId,
                                     String adMobUnitId,
                                     boolean isShowCloseButton,
                                     Context context,
                                     Boolean isTest,
                                     CTAdEventListener adListener) 
    /**
     * Show interstitial advertisement
     *
     * @param adView The advertisement container which return by preload
     */
    public static void showInterstitial(CTNative adView)

    /**
     * Close the interstitial advertisement
     *
     * @param adView The advertisement container which return by preload
     */
    public static void closeInterstitial(CTNative adView) 

    
```


###### CTNative: A framelayout container view which hold the advertisement.

```
    /**
     * Get all error logs which return by the advertisement query process.
     * @return Error List
     */
    public List<CTError> getErrors()

    /**
     * Is current advertisement load successfully.
     * @return
     */
    public boolean isLoaded()
```


###### CTAdEventListener: Call back interface for the advertisement loading process.

```
    /**
     * Load and render advertisement successful.
     */
    public void onAdviewGotAdSucceed(CTNative result);

    /**
     * Load and render interstitial advertisement successful.
     */
    public void onInterstitialLoadSucceed(CTNative result);

    /**
     * Load and render advertisement failed.
     */
    public void onAdviewGotAdFail(CTNative result);

    /**
     * Advertisement landing page will show.
     */
    public void onAdviewIntoLandpage(CTNative result);

    /**
     * Try to go to advertisement landing page, but failed.
     */
    public void onStartLandingPageFail(CTNative result);

    /**
     * User left the advertisement landing page. 
     */
    public void onAdviewDismissedLandpage(CTNative result);

    /**
     * User click the advertisement.
     */
    public void onAdviewClicked(CTNative result);

    /**
     * User close the advertisement.
     */
    public void onAdviewClosed(CTNative result);

    /**
     * When the advertisement has been destroyed or garbage collected.
     */
    public void onAdviewDestroyed(CTNative result);
```

###<a name="errorcode">Error Code From SDK</a>：

| Erro Code               | Description                   |
| ------------------ | -------------------- |
| ERR\_000\_TRACK  | Track exception              |
| ERR\_001\_INVALID_INPUT     | Invalid parameter            |
| ERR\_002\_NETWORK | Network exception                 |
| ERR\_003\_REAL_API     | Error from Ad Server            |
| ERR\_004\_INVALID_DATA     | Invalid advertisement data                |
| ERR\_005\_RENDER_FAIL     | Advertisement render failed               |
| ERR\_006\_LANDING_URL     | Landing failed         |
| ERR\_007\_TO_DEFAULT_MARKET     | Implicitly lading failed            |
| ERR\_008\_DL_URL     | Deep-Link exception        |
| ERR\_009\_DL_URL_JUMP     | Deep-Link jump exception        |
| ERR\_010\_APK_DOWNLOAD_URL     | Application package download failed            |
| ERR\_011\_APK_INSTALL     | Application install failed                |
| ERR\_012\_VIDEO_LOAD     | Load the video exception              |
| ERR\_013\_PAGE_LOAD     | Load the html5 page failed               |
| ERR\_014\_JAR_UPDATE_VERSION     | Check the update jar failed        |
| ERR\_015\_GET_GAID     | Cannot get google advertisement id failed |
| ERR\_016\_GET_AD_CONFIG     | Cannot get the account configuration or template |
| ERR\_017\_INTERSTITIAL_SHOW_NO_AD     | Try to load the interstitial advertisement, but the advertisement is not ready  |
| ERR\_999\_OTHERS     | All other errors  |
|                    |                      |


###<a name="sample">Sample code</a>

#### Banner advertisement：
```
    public void loadAd() {
        // Create a callback to monitor the advertisement loading process(Implmenting the CTAdEventListener interface)
        MyCTAdEventListener listener = new MyCTAdEventListener();
        // Try to get a banner advertisement.
        CTNative res = CTService.getBanner(Config.slotIdBanner,
                Config.fbId, Config.ADMOB_APP_ID,
                Config.ADMOB_AD_UNIT_ID_BANNER, true, ContextHolder.getContext(),
                true, listener);

        adView = res;
    }

```

#### Interstitial advertisement：
```
    public void loadAd() {
        // Create a callback to monitor the advertisement loading process(Implmenting the CTAdEventListener interface)
        MyCTAdEventListener listener = new MyCTAdEventListener();
        // Preload an interstitial advertisement.
        CTNative res = CTService.preloadInterstitial(Config.slotIdInterstitial,
                Config.fbId,
                Config.ADMOB_APP_ID, Config.ADMOB_AD_UNIT_ID_INTERSTITIAL,
                true, ContextHolder.getContext(), true, listener);
        adView = res;
    }
    
```

#### Native advertisement：

```
    public void loadAd() {
        // Create a callback to monitor the advertisement loading process(Implmenting the CTAdEventListener interface)
        MyCTAdEventListener listener = new MyCTAdEventListener();
        // Try to get a native advertisement
        CTNative res = CTService.getNative(Config.slotIdNative, Config.fbId,
                Config.ADMOB_APP_ID, Config.ADMOB_AD_UNIT_ID_NATIVE, true,
                ContextHolder.getContext(), true, listener);
        adView = res;
    }

```

###<a name="reference">How to apply Facebook/Admob advertisement</a>：
* [Apply Facebook advertisement](https://developers.facebook.com/docs/audience-network)
* [Apply Google Admob advertisement](https://firebase.google.com/docs/admob/android/quick-start)