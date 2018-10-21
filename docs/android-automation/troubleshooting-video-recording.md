---
layout: default
title:  "Troubleshooting- Video Recording"
excerpt: "Learn how to use Bellatrix cross-platform video recording."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/troubleshooting-video-recording/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```csharp
[TestClass]
[VideoRecording(VideoRecordingMode.OnlyFail)]
[Android(Constants.AndroidNativeAppPath,
    Constants.AndroidDefaultAndroidVersion,
    Constants.AndroidDefaultDeviceName,
    Constants.AndroidNativeAppAppExamplePackage,
    ".view.Controls1",
    AppBehavior.ReuseIfStarted)]
public class VideoRecordingTests : AndroidTest
{
    [TestMethod]
    public void ButtonClicked_When_CallClickMethod()
    {
        var button = App.ElementCreateService.CreateByIdContaining<Button>("button");

        button.Click();
    }

    [TestMethod]
    [VideoRecording(VideoRecordingMode.DoNotRecord)]
    public void ButtonClicked_When_CallClickMethodSecond()
    {
        var button = App.ElementCreateService.CreateByIdContaining<Button>("button");

        button.Click();
    }
}
```

Explanations
------------
```csharp
[VideoRecording(VideoRecordingMode.OnlyFail)]
```
This is the attribute for cross-platform video recording by Bellatrix. The engine checks after each test, its result, depending on the specified video saves the video.
All video recording modes:
- **Always** - records and save video for all tests.
- **DoNotRecord** - wont' record any videos.
- **Ignore** - ignores the tests.
- **OnlyPass** - saves the videos only for pass tests.
- **OnlyFail** - saves the videos only for failed tests.
If you place attribute over the class all tests inherit the behaviour.
```csharp
[TestMethod]
[VideoRecording(VideoRecordingMode.DoNotRecord)]
public void ButtonClicked_When_CallClickMethodSecond()
```
It is possible to put it over each test and this way you override the class behaviour only for this particular test. The global behaviour for all tests in the class is to save the videos only for failed tests. Only for this particular test, we tell Bellatrix not to make a video.

Configuration
-------------
If you open the **testFrameworkSettings.json** file, you find the **videoRecordingSettings** section that controls this behaviour.
```json
"videoRecordingSettings": {
       "isEnabled": "true",
        "waitAfterFinishRecordingMilliseconds": "500",
        "filePath": "C:\\Troubleshooting\\Videos"
}
```
You can turn off the making of videos for all tests and specify where the videos to be saved. **waitAfterFinishRecordingMilliseconds** adds some time to the end of the test, making the video not going black immediately. In the extensibility chapters read more about how you can create custom video recorder or change the saving strategy.