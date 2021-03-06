---
layout: default
title:  "Extensability- Extend Existing Elements- Child Elements"
excerpt: "Learn how to extend BELLATRIX iOS elements using child elements."
date:   2021-11-23 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/extensibility-extend-existing-elements-child-elements/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```csharp
[TestFixture]
public class ExtendExistingElementWithChildElementsTests : IOSTest
{
    [Test]
    public void ButtonClicked_When_CallClickMethod()
    {
        var button = App.Components.CreateByName<ExtendedButton>("ComputeSumButton");

        button.SubmitButtonWithScroll();
    }
}
```

Explanations
------------
```csharp
public class ExtendedButton : Button
{
    public void SubmitButtonWithScroll()
    {
         this.ToExists().ToBeClickable().WaitToBe();
         ScrollToVisible(ScrollDirection.Down);
         Click();
    }
}
```
The second way of extending an existing element is to create a child component. Inherit the element you want to extend. In this case, a new method is added to the standard **Button** component. Next in your tests, use the **ExtendedButton** instead of regular **Button** to have access to this method. The same strategy can be used to create a completely new element that BELLATRIX does not provide.
    // You need to extend the 'Element' as a base class.
```csharp
var button = App.Components.CreateByName<ExtendedButton>("ComputeSumButton");
```
Instead of the regular button, we create the **ExtendedButton**, this way we can use its new methods.
```csharp
button.SubmitButtonWithScroll();
```
Use the new custom method provided by the **ExtendedButton** class.