After you set up `ScanbotCameraView` next logical step would be to start using contour detection and draw results on screen.

#### Some background

`ScanbotCameraView` has a method `getPreviewBuffer()` which allows you to register for preview frames from the camera. While you can implement your own smart features Scanbot SDK comes with built-in `ContourDetectorFrameHandler` which performs contour detection and outputs results to listeners.

#### Contour detection

To start contour detection, you have to attach `ContourDetectorFrameHandler` to preview buffer:

    ScanbotCameraView cameraView = (ScanbotCameraView) findViewById(R.id.cameraView);

    ContourDetectorFrameHandler frameHandler = new ContourDetectorFrameHandler((Context) this);
    cameraView.getPreviewBuffer().addFrameHandler(frameHandler);

or even shorter

    ContourDetectorFrameHandler frameHandler = ContourDetectorFrameHandler.attach(cameraView);

At his point contour detection becomes active. Now, all we have to do is listen for results:

    frameHandler.addResultHandler(new ContourDetectorFrameHandler.ResultHandler() {

        @Override
        public boolean handleResult(ContourDetectorFrameHandler.DetectedFrame result) {
            // Handle result as you like
            return false;
        }

    });

#### Drawing detected contour

To draw detected contour use `PolygonView`. First, add it as sub-view of `ScanbotCameraView`:

    <net.doo.snap.camera.ScanbotCameraView
        android:id="@+id/cameraView"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <net.doo.snap.ui.PolygonView
            android:id="@+id/polygonView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:polygonStrokeWidth="8dp"
            app:polygonStrokeColor="#ffffff"
            app:polygonFillColor="#00ff00"/>

    </net.doo.snap.camera.ScanbotCameraView>

Second, `PolygonView` should receive callbacks from `ContourDetectorFrameHandler`:

    PolygonView polygonView = (PolygonView) findViewById(R.id.polygonView);
    frameHandler.addResultHandler(polygonView);

Done. If you'll start the app, polygon will be drawn on the screen.

#### Customizing drawn polygon

`PolygonView` supports following attributes (which you can add in XML, as shown in example above):

* `polygonStrokeWidth` - width (thickness) of polygon lines
* `polygonStrokeColor` - color of polygon lines
* `polygonFillColor` - fill color of polygon