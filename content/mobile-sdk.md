## Android SDK
=======
![](ic_launcher.png)

Everybag Image Matching SDK for Android supports >=4.1 Android devices with a built-in camera from [Google projects](https://github.com/google/cameraview) and Android OS 4.1 and higher.
Everybag Image Matching SDK for Android allows to :

* build easy camera.
* support Camera API 1 for Android < 6.0 and support Camera API 2 for Andorid >= 6.0.
* provides 100% matching for taken pictures products.

For more information please see [everybag.de](http://www.everybag.de/)

**Releases** 

The current release is 0.27.0

### Download

The SDK can be downloaded via Gradle

#### Gradle Code

```json
repositories {
    maven {
        url "https://www.myget.org/F/everybag/maven/"
    }
}

dependencies {
    ...
    compile 'de.everybag:everybag.cameraview:0.27.0'
    compile 'de.everybag:everybag.sdk:0.27.0'
}
```

### Set App key and IAuthCallback:

```java
public class SplashScreen extends Activity implements IAuthCallback
{
    @Override
    public void onCreate(Bundle savedInstanceState) {
        ...
        //Set
        Everybag
        .getInstance()
        //Enabling crash report will keep us updated in case you face issues with our SDK
        .enableCrashReport(true)
        .setAuthCallback(this)
        .login(this, client_id, client_secret);
    }

    @Override
    public void onSuccess(AccessToken accessToken) {
        //When it's success login, AccessToken is managed by Everybag SDK you don't need to save it
        Intent intent = new Intent(SplashScreen.this, MainActivity.class);
        startActivity(intent);
        finish();
    }

    @Override
    public void onError(ResponseError error) {
        //When it's error
    }
}
```

### Integrate Everybag Camera

With Everybag Custom Camera SDK you can integrate camera just in few code lines
```xml
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <de.everybag.cameraview.CameraView
        android:id="@+id/camera"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:adjustViewBounds="true"
        app:imageSize="512x512"
        app:imageQuality="60"
        app:autoFocus="true"
        app:barcode="false"
        app:aspectRatio="16:9"
        app:facing="back"
        app:flash="auto"/>
</FrameLayout>
```

```java
public class MainActivity extends Activity implements Callback {
    private CameraView camera;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        ...
         camera = (CameraView) findViewById(R.id.camera);
                                camera.addCallback(this);
                                camera.start();
        ...
    }

    //when app resuemd
    @Override
    protected void onResume() {
        super.onResume();
        camera.start();
    }

    //when lock screen or App paused
    @Override
    protected void onPause() {
        super.onPause();
        camera.stop();
    }

    //Android >=6.0 permissions
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        if (requestCode == ICamera.REQUEST_CAMERA_PERMISSION) {
            if (grantResults.length == 1 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //On Permission OK
            }
            else
            {
                //Permission Deny
            }
        }
    }

   @Override
   public void onCameraOpened(CameraView cameraView) {
        //On Camera Opened
   }

   @Override
   public void onCameraClosed(CameraView cameraView) {
        //On Camera Closed
   }

   @Override
   public void onPictureTaken(CameraView cameraView, byte[] data) {
       //On Picture Taken
   }

   @Override
   public void onError(CameraView cameraView, String errorMessage) {
       //On Camera Error
   }
    ...
}
```

### Match Taken Pictures

Demo sample to match taken pcitures
```java
public class MainActivity extends Activity implements Callback {
    public void onCreate(Bundle savedInstanceState) {
        ...
        View btnTakePicture = findViewById(R.id.btnTakePicture);
        btnTakePicture.setOnClickListener(new OnClickListener() {
            public void onClick(View v) {
                camera.takePicture();
            }
        });
    }

    //On taken picture from Everybag Camera
    @Override
    public void onTakenPicture(byte[] image) {
        camera.stop();
        Intent intent = new Intent(MainActivity.this, SearchResultActivity.class);
        intent.putExtra("image", image);
        startActivity(intent);
    }
    ...
}

public class SearchResultActivity extends BaseActivity implements IMatchCallback {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    ...
        byte[] image = getIntent().getExtras().getByteArray("image");
        Everybag.getInstance()
                .setMatchCallback(this)
                .match(image);
    }
    @Override
    public void onMatched(List<OfferInfo> offerInfos) {
        //onMatched List offers object
    }

    @Override
    public void onMatched(JSONObject jsonObject) {
        //onMatched JsonObject
    }

    @Override
    public void onMatched(String json) {
        //onMatched String Json
    }

    @Override
    public void onError(ResponseError s) {
        //On Error
    }
}
```

### Clear running or pending tasks

To clear pending or running background tasks
```java
public class MainActivity extends Activity
{
    ...
    @Override
    protected void onDestroy() {
        super.onDestroy();
        Everybag.getInstance().clearAllTasks();
    }
}
```