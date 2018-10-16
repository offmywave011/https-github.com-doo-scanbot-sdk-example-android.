## What is the latest version of the Scanbot SDK for Android?

The current version of the SDK is **1.38.1**


## Release History and Changelog

##### 1.38.1
- 🚀 Bug fixes: 
  * Fixed a bug with freezing camera on Activity start
  * Fixed "CroppingActivity" layout for "ready-to-use-ui".

##### 1.38.0
- 🚀 Improvements: 
  * Updated jpeg-turbo lib version to 1.5.3

##### 1.37.0
- 🎉 NEW:
  * Reset/Detect functionality in RTU Cropping UI
  * Added support for `orientationLockMode` in RTU Cropping UI
- ⚠️ Breaking changes:
  * The config parameter `pageCounterButtonTitle` in RTU Document Scanner UI now requires a placeholder "%d" for the number of pages (e.g. `pageCounterButtonTitle: "%d Page(s)"`)
- 🐞 Bug fixes:
  * Fixed an issue with camera on "Xiaomi Redmi 5 Plus" devices with MIUI Chinese ROM ([#72](https://github.com/doo/scanbot-sdk-example-android/issues/72))

##### 1.36.0
- 🚀 Improvements: 
  * MRZ Recognizer: improved detection on still images and parsing of some optional MRZ fields.
  * Check out the updated [mrz-scanner](https://github.com/doo/scanbot-sdk-example-android/tree/master/ScanbotSDKexample/mrz-scanner) and [ready-to-use-ui](https://github.com/doo/scanbot-sdk-example-android/tree/master/ScanbotSDKexample/ready-to-use-ui) example apps.
- ⚠️ Breaking changes: 
  * All MRZ Scanner and Recognizer components (`MRZScanner`, `MRZScannerActivity`) now require an additional trained data blob file (MRZ cascade blob file `mrz.xml`), which is included in the SDK `io.scanbot:sdk-package-3` and can be fetched via `blobManager.fetch(blobFactory.mrzCascadeBlob(), false)`!

##### 1.35.0
- 🎉 NEW: 
  * Cheque Scanner - Real-time extraction of account & routing number (check out the [cheque-scanner](https://github.com/doo/scanbot-sdk-example-android/tree/master/ScanbotSDKexample/cheque-scanner) example app)
  * Added a new config parameter `rotateButtonHidden` for the **RTU UI** `CroppingActivity`
- ⚠️ Breaking change: Added file format extension (.jpg or .png) for **RTU UI** `Page` images: 
  * Affects the image files created by all **RTU UI** components, like `DocumentScannerActivity`, `CroppingActivity`, etc).
  * Please note that only the **new created** image files will contain extensions. The **currently available** image files in the temporary storage of the Scanbot SDK 
    **will not get** file extensions and may become inaccessible. So please make sure to implement a suitable migration mechanism.
  * If you need backwards compatibility, you can disable the file format extensions via `PageStorageSettings.addImageFileFormatExtension(false)` on initialization of the SDK (`new ScanbotSDKInitializer().usePageStorageSettings(new PageStorageSettings.Builder().addImageFileFormatExtension(false).build())...`).
- 🐞 Bug fixes:
  * MRZ Recognizer: fixed extraction of the field `dateOfBirth` from some French ID cards
- 🚀 Improvements:
  * Updated Google Vision lib version to 15.0.2
  * Improvements and potential fixes in SDK license manager

##### 1.34.0
* Updated OpenCV version to 3.4.2
* DC Scanner:
  - Improved recognition of low-constrast scans
  - ⚠️ Breaking change: The DC Scanner now uses `eng.traineddata` instead of `deu.traineddata`!

##### 1.33.3
* Removed `allowBackup` flag in AndroidManifest.xml in `io.scanbot:sdk-package-ui` library

##### 1.33.2
* Minor fixes and improvements in `EditPolygonImageView`

##### 1.33.1
* DC scanner: improved date recognition in cases where the date text overlaps the date field's grid
* Minor fixes and improvements in `EditPolygonView`

##### 1.33.0
* Extended and improved the API to apply image filter on pages coming from the Ready-To-Use UI.
* Added new image filters:
  - `IMAGE_FILTER_BACKGROUND_CLEAN` - Cleans up the background and tries to preserve photos within the image.
  - `IMAGE_FILTER_BLACK_AND_WHITE` - Black and white filter with background cleaning. Creates a grayscaled 8-bit image with mostly black or white pixels.
* Minor bug fixes and improvements.

##### 1.31.3
* Several fixes in `io.scanbot.sdk.ScanbotSDKInitializer` and `io.scanbot.sdk.ScanbotSDK`

##### 1.31.2
* Several fixes in `io.scanbot.sdk.ScanbotSDKInitializer` and `io.scanbot.sdk.ScanbotSDK`

##### 1.31.1
* Added functionality to apply a real orientation lock in `ScanbotCameraView`. Use the new methods `cameraView.lockToLandscape(boolean lockPicture)` or `cameraView.lockToPortrait(boolean lockPicture)` to lock the UI as well as the taken picture to a desired orientation.
* Fixed a bug with shutter sound in `ScanbotCameraView` on some Android devices (e.g. Xiaomi).
* Fixed a bug with freezing camera on Huawei P20 Lite.

##### 1.31.0
* Added native libs for `arm64-v8a`.
  * ⚠️ **Please note:** In August 2019, Google Play Store will require that new apps and app updates with native libraries provide 64-bit versions in addition to their 32-bit versions. (https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)
  * We strongly recommend to upgrade the Scanbot SDK for Android to the latest version to benefit from the full support of `arm64-v8a` libs.
* Upgraded Dagger to version 2.16. It is recommended to upgrade the Dagger version in your App to 2.14.x or higher.

##### 1.30.0
* 🎉 NEW! Added **Ready-To-Use UI** Components: `DocumentScannerActivity`, `CroppingActivity`, `MRZScannerActivity`, `BarcodeScannerActivity`. A set of easy to integrate and customize high-level UI components for the most common tasks in Scanbot SDK.
  * Published a new package `io.scanbot:sdk-package-ui` containing the new **Ready-To-Use UI** Components.
  * Added a new demo project `ready-to-use-ui` containing example code for the new **Ready-To-Use UI** Components.
* Added a beautiful, animated ShutterButton component (`io.scanbot.sdk.ui.camera.ShutterButton`) which can also be used with our **Classical UI Components**. Check out the [camera-view](https://github.com/doo/Scanbot-SDK-Examples/tree/master/ScanbotSDKexample/camera-view) example app. 
* Minor bug fixes and improvements.
* Internal refactorings and improvements:
  * Deprecated some classes in the `net.doo.snap` Java package. Please use the equivalent classes from the new Java package `io.scanbot.sdk`. 
    (e.g. `net.doo.snap.ScanbotSDKInitializer` => `io.scanbot.sdk.ScanbotSDKInitializer`,  `net.doo.snap.ScanbotSDK` => `io.scanbot.sdk.ScanbotSDK`, etc.)

##### 1.28.7
* Added `setIgnoreBadAspectRatio(boolean ignoreBadAspectRatio)` method for the `AutoSnappingController`.
* Improved `AutoSnappingController` precision.

##### 1.28.6
* Updated Google Mobile Vision library version to 12.0.1.
* Optimized `ScanbotCameraView` performance.

##### 1.28.5
* Added new attribute `magnifierEnableBounding` to `MagnifierView`, which allowing to enable/disable bounding of `MagnifierView` to the edge of screen.
* Fixed issue with paddings on a `MagnifierView`

##### 1.28.4
* `DCScanner` detection quality improvements.

##### 1.28.3
* Scanbot SDK could be reinitialised with the new SDK license.
* `MagnifierView` should not be the same size as `EditPolygonImageView`, but have to have the same aspect ratio.

##### 1.28.2
* Removed deprecated attributes `magnifierCrossSize`, `magnifierCrossStrokeWidth`, `magnifierStrokeWidth` and `editPolygonMagnifier` from `MagnifierView` since they don't have any effects on the view.
To customize the `MagnifierView` please use the attributes `magnifierImageSrc`, `magnifierRadius` and `magnifierMargin`.

##### 1.28.1
* Exposed the method `setImageRotation()` in `MagnifierView`.

##### 1.28.0
* Added confidence values for detected fields in `MRZScanner`.
* Changed default barcode detector to ZXing barcode detector.
* Added Finder feature for barcode detectors.

##### 1.27.3
* Updated Google Mobile Vision library version to 12.0.0.

##### 1.27.2
* Fixed native libraries loading.
* Fixed `DCScanner` bugs.

##### 1.27.1
* Added `TIFFWRiter` feature to the Scanbot SDK package 1.
* Added new "Pure Binarization" (`ContourDetector.IMAGE_FILTER_PURE_BINARIZED`) filter.
* Added `DCScanner` recognition methods for `Bitmap`s and JPEG images.

##### 1.27.0
* Added `MRZScanner` (Machine Readable Zone scanner) feature to the Scanbot SDK package 3.
* Added Finder feature for `MRZScanner`.
* Added `DCScanner` (Disability Certificate scanner) feature to the Scanbot SDK package 4.
* Updated Tesseract version to 3.05.01.
* Updated Android support library dependencies version to 27.0.2.

##### 1.26.7
* Minor bug fixes and improvements

##### 1.26.6
* Minor bug fixes

##### 1.26.5
* Added `cameraView.setPreviewMode(CameraPreviewMode mode)` for adjusting camera preview frames in the camera layout.
* Minor bug fixes

##### 1.26.4
* Minor bug fixes

##### 1.26.2
* Fixed bug when `cameraView.setShutterSound(boolean enable)` throws RuntimeException if the camera is already released.

##### 1.26.1
* Updated transitive Scanbot SDK dependencies
* Added `cameraView.setShutterSound(boolean enable)` and `cameraView.setAutoFocusSound(boolean enable)` methods in `ScanbotCameraView`
* Optimized `AutoSnappingController` sensitivity and auto snapping timings

##### 1.26.0
* Optimized Scanbot SDK license check - added Java exceptions with human readable messages
* Added `ScanbotSDK#isLicenseValid()` method for the license validation
* Fixed bug when the image preview edge zooming in `MagnifierView` was displayed incorrectly

##### 1.25.2
* Added detected text paragraphs, lines and words blocks of the optical character recognitions result in `OcrResult`
* Added `PageFactory#buildPage(Bitmap image)` and `PageFactory#buildPage(byte[] image)` methods for `Page` generation

##### 1.25.1
* Added rotation methods for `EditPolygonImageView`

##### 1.25.0
* Added Barcode/QR code scanner feature to the Scanbot SDK package 1

##### 1.24.0
* Optimized transitive dependencies for Scanbot SDK library
* Stop supporting `armeabi` architecture

##### 1.23.3
* Fixed contour detection and autosnapping freeze on the autofocus tap

##### 1.23.2
* Added `detectionScore` value in `DetectedFrame` class
* Fixed 180 degree camera preview rotation

##### 1.23.0
* Added Scanbot SDK package 3 with SEPA Pay Form scanner feature

##### 1.22.6
* Fixed bug when `ScanbotCameraView` crashes with `IllegalStateException` after `onPause`

##### 1.22.5
* Added methods for setting contour detector parameters in `ContourDetectorFrameHandler`

##### 1.22.4
* Added methods for setting OCR and language classifier blobs paths in `ScanbotSDKInitializer`
* Updated version of transitive dependencies

##### 1.22.3
* Added continuous focus option for `ScanbotCameraView`

##### 1.22.2
* Added method that provides internal OCR blobs directory in `BlobManager` in SDK-2

##### 1.22.1
* Added methods for performing OCR with multiple predefined languages in `TextRecognition` in SDK-2

##### 1.22.0
* Scanbot SDK was switched from RoboGuice DI to Dagger 2

##### 1.21.3
* Fixed minor camera issues
* Added setters for edge width and color in `EditPolygonImageView`

##### 1.21.2
* Fixed dependencies in SDK packages

##### 1.21.1
* Added camera preview and picture size setters in `ScanbotCameraView`
* Added camera orientation locks in `ScanbotCameraView`

##### 1.19.0
* Fixed issue when `EditPolygonImageView` with `MagnifierView` was working only as part of an `Activity`.
* Removed `DrawMagnifierListener`.

##### 1.18.4
* `EditPolygonImageView` was not working properly on Android Nougat.

##### 1.18.2
* It was not possible to create PDFs without embedded text in SDK-2

##### 1.18.1
* Fixed crash related to text recognition

##### 1.18.0
* It's now possible to perform OCR without sandwiched PDF as part of result

##### 1.17.0
* It's now possible to customize the `Logger` implementation used by the SDK.

##### 1.16.0
* Added `ContourDetector.processImageAndRelease` - more memory efficient version of `ContourDetector.processImageF`.

##### 1.15.3
* Fixed build issues of OCR example.

##### 1.15.1
* Removed uses-feature android.hardware.camera. Camera is now optional.

##### 1.14.0
* Removed unused dependencies. Removed permission declarations from Package 1. Users are now responsible for declaring permissions. For more information see [this page](https://github.com/doo/Scanbot-SDK-Examples/wiki/Permissions).

##### 1.13.1
* Fixed bug when final image contained less than preview image from camera stream.

##### 1.13.0
* Added `ImageQualityOptimizer` which allows you to optimize image size.

##### 1.12.2
* Fixed bug when `AutosnappingController` stop operating after `onPause`.

##### 1.12.1
* Color-document filter was disabled in `ContourDetector`. Now it works again.

##### 1.12.0
* Added `ScanbotSDKInitializer#withLogging()` method which allows you to turn on logging for SDK (disabled by default).
* Improved edge detection.
* Added new filter to `ContourDetector`.

##### 1.11.0
* Added `ScanbotSDK.isLicenseActive()` method.
