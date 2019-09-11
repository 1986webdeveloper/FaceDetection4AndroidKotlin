# FaceDetection4AndroidKotlin

This is face detection library for Android with Java code.

It is easy to implement face detection With Firebase ML Kit's face detection API, you can detect faces in an image, identify key facial features, and get the contours of detected faces.

You can detect following things with ML Kit's face detection API
- Face
- Smiling probability
- Nose
- Left Eye and Right Eye
- Left Eyebrow top/bottom and Right Eyebrow top/bottom
- Upper Lip Top and Upper Lip Bottom and Lower Lip Top and Lower Lip Bottom
- Left Cheek and Right Cheek
- Left Ear and Right Ear
- FaceContour

<b>Usage</b>
1 Add below code in app level gradle
```gradle
implementation 'com.google.firebase:firebase-ml-vision:23.0.0'
implementation 'com.google.firebase:firebase-ml-vision-face-model:18.0.0'
```
2 You need to add FaceDetectionProcessor in your activity. Add below code in your FaceDetectionActivity
```java
cameraSource.setMachineLearningFrameProcessor(new FaceDetectionProcessor(getResources()));
```
3 draw method is used to draw face inside FaceGraphic class
```Kotlin
    override fun draw(canvas: Canvas) {
        val face = firebaseVisionFace ?: return

        // Draws a circle at the position of the detected face, with the face's track id below.
        // An offset is used on the Y axis in order to draw the circle, face id and happiness level in the top area
        // of the face's bounding box
        val x = translateX(face.boundingBox.centerX().toFloat())
        val y = translateY(face.boundingBox.centerY().toFloat())
        canvas.drawCircle(x, y - 4 * ID_Y_OFFSET, FACE_POSITION_RADIUS, facePositionPaint)
        canvas.drawText("id: " + face.trackingId, x + ID_X_OFFSET, y - 3 * ID_Y_OFFSET, idPaint)
        canvas.drawText(
            "happiness: " + String.format("%.2f", face.smilingProbability),
            x + ID_X_OFFSET * 3,
            y - 2 * ID_Y_OFFSET,
            idPaint
        )
        if (facing == CameraSource.CAMERA_FACING_FRONT) {
            canvas.drawText(
                "right eye: " + String.format("%.2f", face.rightEyeOpenProbability),
                x - ID_X_OFFSET,
                y,
                idPaint
            )
            canvas.drawText(
                "left eye: " + String.format("%.2f", face.leftEyeOpenProbability),
                x + ID_X_OFFSET * 6,
                y,
                idPaint
            )
        } else {
            canvas.drawText(
                "left eye: " + String.format("%.2f", face.leftEyeOpenProbability),
                x - ID_X_OFFSET,
                y,
                idPaint
            )
            canvas.drawText(
                "right eye: " + String.format("%.2f", face.rightEyeOpenProbability),
                x + ID_X_OFFSET * 6,
                y,
                idPaint
            )
        }

        // Draws a bounding box around the face.
        val xOffset = scaleX(face.boundingBox.width() / 2.0f)
        val yOffset = scaleY(face.boundingBox.height() / 2.0f)
        val left = x - xOffset
        val top = y - yOffset
        val right = x + xOffset
        val bottom = y + yOffset
        canvas.drawRect(left, top, right, bottom, boxPaint)

        // draw landmarks
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_BOTTOM)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_CHEEK)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_EAR)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_LEFT)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.LEFT_EYE)
        drawBitmapOverLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.NOSE_BASE)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_CHEEK)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_EAR)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.RIGHT_EYE)
        drawLandmarkPosition(canvas, face, FirebaseVisionFaceLandmark.MOUTH_RIGHT)
    }
```

<b>Output:</b>

![alt text](https://github.com/1986webdeveloper/FaceDetection4AndroidKotlin/blob/master/ezgif-4-72e974aee956.gif)

