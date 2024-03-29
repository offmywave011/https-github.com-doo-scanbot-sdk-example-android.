The Android camera API might seem to be very tricky and far from being developer-friendly (in fact, very far). To help you avoid the same issues which we have encountered while developing Scanbot, we created the `ScanbotCameraView`.

### Getting Started

`ScanbotCameraView` is available with the SDK Package 1. To get started, you have to undertake 3 steps.

First: Add this permission to the AndroidManifest.xml

    <uses-permission android:name="android.permission.CAMERA" />

Second: Add it to your layout, which is as simple as:

    <net.doo.snap.camera.ScanbotCameraView
            android:id="@+id/camera"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

Third: Delegate `onResume` and `onPause` methods of your `Activity` (or `Fragment`, whatever you are using) to `ScanbotCameraView`:

    public class MyActivity extends Activity {

        // ...

        @Override
        public void onResume() {
            super.onResume();
            scanbotCameraView.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            scanbotCameraView.onPause();
        }

    }

That is it! You can start your app and you should see the camera preview.


### Preview Mode

The `ScanbotCameraView` supports 2 preview modes:
* `CameraPreviewMode.FIT_IN` - in this mode camera preview frames will be downscaled to the layout view size. Full preview frame content will be visible, but unused edges could be appeared in the preview layout.
* `CameraPreviewMode.FILL_IN` - in this mode camera preview frames fill the layout view. The preview frames may contain additional content on the edges that was not visible in the preview layout.

By default, `ScanbotCameraView` uses `FILL_IN` mode. You can change it using `cameraView.setPreviewMode(CameraPreviewMode mode)` method.


### Auto-focus Sound and Shutter Sound

You can enable/disable auto-focus event system and shutter sounds using setters in `ScanbotCameraView`.

    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
            @Override
            public void onCameraOpened() {
                cameraView.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        cameraView.setAutoFocusSound(false);
                        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
                            cameraView.setShutterSound(false);
                        }
                    }
                }, 700);
            }
        });
    
`cameraView.setShutterSound(boolean enabled)` set the camera shutter sound state. By default - `true`, the camera plays the system-defined camera shutter sound when takePicture() is called.

Note that devices may not always allow disabling the camera shutter sound. If the shutter sound state cannot be set to the desired value, this method will be ignored.
https://developer.android.com/reference/android/hardware/Camera.html#enableShutterSound(boolean)

Also, it is supported only with Android API 17+. 

### Continuous Focus Mode

For most use cases it is recommended to enable the "Continuous Focus Mode" of the Camera. Use the `continuousFocus()` method of `ScanbotCameraView` for this. It should be called from the main thread and only when camera is opened (CameraOpenCallback):

    cameraView = (ScanbotCameraView) findViewById(R.id.camera);
    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
        @Override
        public void onCameraOpened() {
            cameraView.postDelayed(new Runnable() {
                @Override
                public void run() {
                    cameraView.continuousFocus();
                }
            }, 700);
        }
    });

**Please note:** The Continuous Focus Mode will be automatically disabled

- after the `autoFocus` method call, 
- after a tap on the `ScanbotCameraView` to perform the autoFocus, 
- or after the `takePicture` event.

In these cases you have to call the `continuousFocus()` method again to re-enable the Continuous Focus Mode.

Example for the `takePicture` event, handling in the `onPictureTaken(..)` method:

    @Override
    public void onPictureTaken(byte[] image, int imageOrientation) {
        // image processing ...
        // ....

        cameraView.post(new Runnable() {
            @Override
            public void run() {
                cameraView.continuousFocus();
                cameraView.startPreview();
            }
        });
    }


### Orientation Lock
By default the `ScanbotCameraView` will create pictures with orientation based on the current device orientation. It is important to understand that the orientation of the taken picture is independent of the locked orientation mode of the `Activity`!

For example: if you just lock the `Activity` to portrait mode, the orientation of the taken image will still be based on the current device orientation!

Since version **1.31.1** the Scanbot SDK provides the functionality to apply a real orientation lock in `ScanbotCameraView`. You can use the new methods `cameraView.lockToLandscape(boolean lockPicture)` or `cameraView.lockToPortrait(boolean lockPicture)` to lock the `Activity` **and** the taken picture to a desired orientation:

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    // ...

    cameraView = (ScanbotCameraView) findViewById(R.id.camera);

    // Lock the orientation of the Activity as well as the orientation of the taken picture to portrait:
    cameraView.lockToPortrait(true);

    // ...
}
```


### Advanced: Preview Size and Picture Size

By default the `ScanbotCameraView` selects the best available picture size (resolution of the taken picture) and a suitable preview size (preview frames).

You can change these values using the setter methods of `ScanbotCameraView`:

    cameraView = (ScanbotCameraView) findViewById(R.id.camera);
    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
        @Override
        public void onCameraOpened() {
            cameraView.stopPreview();

            List<Camera.Size> supportedPictureSizes = cameraView.getSupportedPictureSizes();
            // For demo purposes we just take the first picture size from the supported list!
            cameraView.setPictureSize(supportedPictureSizes.get(0));
            
            List<Camera.Size> supportedPreviewSizes = cameraView.getSupportedPreviewSizes();
            // For demo purposes we just take the first preview size from the supported list!
            cameraView.setPreviewSize(supportedPreviewSizes.get(0));

            cameraView.startPreview();
        }
    });

⚠️ Please take the following into account when changing these values: On most devices the aspect ratio of the camera sensor (camera picture) does not match to the aspect ratio of the display.

