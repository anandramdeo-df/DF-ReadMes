
## Document Scanner SDK
![Download](https://api.bintray.com/packages/datafornix/documentscanner/documentscanner/images/download.svg)

![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)

### This SDK based on OpenCV which is been created to scan, capture and crop the document.

- Capture Document from Front and Back
- Crop with Auto-Edge-Detection

Sole purpose of this SDK is to capture the snapshot of document via scan mode, even you get the functionality of auto-edge-detection, so that you can crop the images more accurately.

#### You can achieve the functionality with easy integration of following steps:
- In your project level `build.gradle` add this dependency:
```sh 
allprojects {
   repositories {
      jcenter()
       maven {
            url "https://mybigdata-df.bintray.com/documentscanner" 
       }
       maven {
            url "https://mybigdata-df.bintray.com/opencvlib"
       }
    }
 } 
```
- In your app level `AndroidMenifest.xml` add this permissions:
```sh 
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```
- In your app level `build.gradle` add this dependency:
```sh
implementation 'documentscanner:documentscanner:1.4.0'
```
- Now in your project's activity create DocumentCaptureInstance object: 
```sh
private lateinit var documentCaptureInstance: DocumentCaptureInstance
```
- Initialise this object with your `sdkToken`
```sh
 documentCaptureInstance = DocumentCaptureInstance(
                activity = this
        )
 documentCaptureInstance.baseUrl = "https://qat.datafornix.com/"
 documentCaptureInstance.sdkToken = "mashreq:kcLk01Twys5WBziFkm55J4oWw43QMQ3t"
```

#### You can customise the colour and theme of the SDK's view according to your project's user-experience.
#### Just access the properties mentioned in the SDK. Have a look at the sample below:

```sh
val config = DocumentCaptureConfig()
config.isDocumentBackCaptureRequired = true
config.cameraScreenBackgroundColor =
       ContextCompat.getColor(this, R.color.black)
config.tutorialScreenBackgroundColor =
       ContextCompat.getColor(this, R.color.colorPrimaryDark)
config.cropScreenBackgroundColor =
       ContextCompat.getColor(this, R.color.black)
```

- Call `captureDocument()` function from `documentCaptureInstance`
```sh
documentCaptureInstance.captureDocument(
       config,
       object : DocumentCaptureInstance.DocumentCaptureListener {
           override fun onDocumentCaptureSuccess(path: List<String>) {
               // get back and front image path from path:List<String>
           }
           override fun onDocumentCaptureFailure(error: DocumentCaptureInstance.DocumentCaptureError) {
               // document uploading failed, please check error
           }
       })
```

- Call `getPassportData()` function from `documentCaptureInstance` to get passport mrz parsed data
```sh
documentCaptureInstance.getPassportData(
       config,
       object : DocumentCaptureInstance.PassportCaptureListener {
           override fun onPassportCaptureSuccess(path: List<String>, passportData: PassportData) {
               // get back and front image path from path:List<String>
               // get parsed data from documentData object like documentData.dateOfBirth
           }
           override fun onPassportCaptureFailure(error: DocumentCaptureInstance.PassportCaptureError) {
               // document uploading failed, please check error
           }
       })
```

- Override `onActivityResult`:
```sh
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
   super.onActivityResult(requestCode, resultCode, data)
   if (resultCode == Activity.RESULT_OK && requestCode == DfDCConstants.REQUEST_CODE_DOCUMENT_CAPTURE_INSTANCE) {
       if (::documentCaptureInstance.isInitialized) {
           documentCaptureInstance.onDocumentCaptureResult(requestCode, resultCode, data)
       }
   }
}
```


#### By calling `captureDocument()` method you will be redirected to the document capture flow which is shown in below sequence of images:
<br>
<p align="left">

<img src="DF-DC-Sceen01.png" width="200"/>
<img src="DF-DC-Sceen02.png" width="200"/>
<img src="DF-DC-Sceen03.png" width="200"/>
<img src="DF-DC-Sceen04.png" width="200"/> <br>
  
#### Repeat the same procedure to capture the back of document as well.
  
<p align="left">
<img src="DF-DC-Sceen05.png" width="200"/>
<img src="DF-DC-Sceen06.png" width="200"/>
<img src="DF-DC-Sceen07.png" width="200"/>
<img src="DF-DC-Sceen08.png" width="200"/>
