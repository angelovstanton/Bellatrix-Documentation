---
layout: default
title:  "Troubleshooting- Screenshots on Fail"
excerpt: "Learn how to generate screenshots on test's fail."
date:   2018-11-22 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/troubleshooting-screenshots-on-fail/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```csharp
[TestClass]
[ScreenshotOnFail(true)]
[IOS(Constants.IOSNativeAppPath,
    Constants.IOSDefaultVersion,
    Constants.IOSDefaultDeviceName,
    AppBehavior.RestartEveryTime)]
public class ScreenshotsOnFailTests : IOSTest
{
    [TestMethod]
    public void ButtonClicked_When_CallClickMethod()
    {
        var button = App.ElementCreateService.CreateByName<Button>("ComputeSumButton");

        button.Click();
    }

    [TestMethod]
    [ScreenshotOnFail(false)]
    public void ButtonClicked_When_CallClickMethodSecond()
    {
        var button = App.ElementCreateService.CreateByName<Button>("ComputeSumButton");

        button.Click();
    }
}
```

Explanations
------------
```csharp
[ScreenshotOnFail(true)]
```
This is the attribute for automatic generation of app screenshots by Bellatrix. The engine checks after each test, its result, if failed, makes the screenshots. If you place attribute over the class all tests inherit the behaviour.
```csharp
[TestMethod]
[ScreenshotOnFail(false)]
public void ButtonClicked_When_CallClickMethodSecond()
```
It is possible to put it over each test and this way you override the class behaviour only for this particular test. The global behaviour for all tests in the class is to make screenshots on fail. Only for this particular test, we tell Bellatrix not to make screenshots.

Configuration
-------------
If you open the **testFrameworkSettings.json** file, you find the screenshotsSettings section that controls this behaviour.
```json
"screenshotsSettings": {
    "isEnabled": "true",
    "filePath": "C:\\Troubleshooting\\Screenshots"
}
```
You can turn off the making of screenshots for all tests and specify where the screenshots to be saved.
In the extensibility chapters read more about how you can create different screenshots engine or change the saving strategy.