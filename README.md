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


**3)fragment (or activity)**
```
package com.package_name;

public class Fragment_Home_Profile extends Fragment {


    private static final int REQUEST_SELECT_PICTURE = 0x01;
    private static final String SAMPLE_CROPPED_IMAGE_NAME = "SampleCropImage";

    public Fragment_Home_Profile() {
        // Required empty public constructor

    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(final LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

        BtnSelectImage.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                Intent intent = new Intent();
                intent.setType("image/*");
                intent.setAction(Intent.ACTION_GET_CONTENT);
                intent.addCategory(Intent.CATEGORY_OPENABLE);
                startActivityForResult(Intent.createChooser(intent, "dali"), REQUEST_SELECT_PICTURE);

            }
        });
        return rootView;
    }

    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data)
    {

        if (resultCode == RESULT_OK) {
            if (requestCode == REQUEST_SELECT_PICTURE) {
                final Uri selectedUri = data.getData();
                if (selectedUri != null) {
                    startCropActivity(data.getData());
                } else {
                    Toast.makeText(getContext(), "cant recive image ...", Toast.LENGTH_SHORT).show();
                }
            } else if (requestCode == UCrop.REQUEST_CROP) 
	    {
	    	//Handle Croped Image ...		
                Uri mUri=UCrop.getOutput(data);
                File userImageFile=new File(mUri.getPath());
		Glide.with(getContext()).load(userImageFile).diskCacheStrategy(DiskCacheStrategy.ALL).fitCenter().into(ivStarImage);

                Send_File_To_Server(userImageFile);

            }
        }
        if (resultCode == UCrop.RESULT_ERROR) {
//            handleCropError(data);
            Toast.makeText(getContext(), "errro crop", Toast.LENGTH_SHORT).show();
        }


    }

    private void startCropActivity(@NonNull Uri uri) {
        String destinationFileName = SAMPLE_CROPPED_IMAGE_NAME+"_"+ System.currentTimeMillis();

        destinationFileName += ".png";


        UCrop uCrop = UCrop.of(uri, Uri.fromFile(new File(getContext().getCacheDir(), destinationFileName)));

        uCrop.withAspectRatio(12, 9)
                .withMaxResultSize(512, 512)
                .start(getContext(),this);
    }


    private void Send_File_To_Server(File userImageFile)
    {
		// ...........
    }



}

```
