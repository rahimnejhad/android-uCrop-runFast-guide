# android-uCrop-runFast-guide

**1)gradle :**
```
compile 'com.github.yalantis:ucrop:2.2.1'
```

**2)manifest**
```
<?xml version="1.0" encoding="utf-8"?>
<manifest package="com.package.name" xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="false"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">

        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="@string/file_provider_authorities"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_provider_paths" />
        </provider>

        <activity
            android:name="com.yalantis.ucrop.sample.SampleActivity"
            android:screenOrientation="portrait">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>

                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

    </application>

</manifest>
```


**3)activity**
