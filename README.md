JGaze
=====

© 2014 Jose Manuel Gómez Poveda.

Eye tracking and gaze library in Java, offering the following functionalities:
* Tracking user features, such as the head, eyes, pupils and eye corners, nose, and mouth
* Blink detection
* Generation of gaze heat maps and visualisations of saccades and fixations
* Supports exporting data for offline analysis
* Abstraction layer for working with video files, images or live video sources in the same way
* WebSockets communication if the Webbit library is included

Using OpenCV for many low-level operations, so the speed is very close to that of implementations in C++.

Execution of JGaze UserMonitor
------------------------------

Click on the image below for launching the UserMonitor application:

[![alt text](http://java.com/js/webstart.png "Launch")](http://jmgomezpoveda.github.io/JGaze/jnlp/JGazeUserMonitor/launch.jnlp)

Note that the sample above needs a webcam in order to capture video.

Also, given that it is not signed, the application will only run if [the security settings in the Java control panel are set to Medium](http://www.java.com/en/download/help/jcp_security.xml).

Sample program
--------------
Sample program that performs face, eye and pupil detection in just a few lines of code:

```java
import org.dreamcoder.jgaze.detectors.FacialFeaturesDetector;
import org.dreamcoder.jgaze.io.SimpleFrameGrabber;
import org.dreamcoder.jgaze.ui.CanvasFrame;
import org.dreamcoder.jgaze.util.OpenCvUtils;
import org.opencv.core.Mat;

public class PupilDetectMin
{
    public static void main(String[] args) throws Exception
    {
        OpenCvUtils.LoadLibrariesDialog();

        SimpleFrameGrabber grabber = new SimpleFrameGrabber(0, 640, 480, 30);

        try {
            Mat origImg = grabber.grab(false);

            FacialFeaturesDetector detector = new FacialFeaturesDetector(origImg.width(), origImg.height()).addPupilDetector();

            CanvasFrame frame = new CanvasFrame("PupilDetectMin", origImg.width(), origImg.height());

            while (frame.isVisible())
            {
                origImg = grabber.grab();

                detector.detect(origImg);

                detector.drawBoundingBoxes(origImg);
                detector.drawEyes(origImg);

                frame.showImage(origImg);
            }

            grabber.stop();
            detector.release();
            frame.dispose();
        }
        catch (Exception e)
        {
            System.out.println("Exception: " + e);
        }
    }
}
```
