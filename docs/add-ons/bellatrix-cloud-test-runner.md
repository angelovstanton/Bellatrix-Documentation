---
layout: default
title:  "BELLATRIX Cloud Test Runner"
excerpt: "Learn to install and use BELLATRIX Cloud Test Runner."
date:   2020-07-23 06:50:17 +0200
parent: add-ons
permalink: /add-ons/cloudtestrunner/
anchors:
  installation: Installation
  usage: Usage
  commands: Commands
  additional-notes: Additional Notes
  what-is-reportportal: What Is ReportPortal?
---
Installation
-------
Open Command Line. Install BELLATRIX Cloud Test Runner with the bellow command.
```
dotnet tool install --global bellatrixcloudtestrunner
```
To update the runner use:
```
dotnet tool update --global bellatrixcloudtestrunner
```
To uninstall the runner use:
```
dotnet tool uninstall --global bellatrixcloudtestrunner
```
Usage
-------
```
bellatrixcloudtestrunner --library="C:\SourceCode\BELLATRIX\Tests\Bellatrix.Web.Tests\bin\Debug\netcoreapp3.1\Bellatrix.Web.Tests.dll" --results="C:\SourceCode\BELLATRIX\Tests\Bellatrix.Web.Tests\bin\Debug\netcoreapp3.1\ciwebresults.trx" --maxCount=4  --retries=2 --threshold=60 --licenseKey="122c5783-d122-4de9-85bf-ss56c5e4554"  --companyName="AutomateThePlanet" --projectName="Bellatrix" --testRunName="CIWebTests" --autoAnalysis --reportPortalProject="bellatrix" --reportPortalAccessToken="asdgr435-66f5-44a3-ghjh5-7956895814ed" --reportPortalUrl="http://myreportportal.com:8080" --reportPortalLaunchName="CIWebTests" --filter="test.FullName.Contains(\"ColorControlBddLoggingTests\")"
```
Commands
-------
```
--library="pathToBuildedFiles\SampleTestProj.dll"
```
Path to the DLL that includes your tests. Make sure all referenced assemblies to be copied to the same folder.
To make sure that this happens, add the following line to your MSBuild project:
```
<PropertyGroup>
  <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
</PropertyGroup>
```
```
--results="pathToResults\result.trx
```
The path where the results produced by the runner will be saved. The file should be in the expected file format. TRX is the file format for .NET tests. If the retry option is turned-on the produced results will be updated automatically.
```
--filter="test.FullName != \"TestName\" AND !test.Categories.Contains(\"CI\")"
```
BELlATRIX Cloud Test Runner has a built-in complex test filter parser, and we can write complex queries to filter the tests.
```
--timeBased
```
Instructs the runner to balance the tests, not based on the count, but on the previous execution times of the tests. To use it, you need to execute all your tests at least one time. This is quite useful because some of your UI tests may execute for 1 minute or more but most of them for 30 seconds or less. As I mentioned, the whole tests execution time is equal to the slowest sub-run. This feature can sometimes drastically decrease the execution time.
```
--retries=3 --threshold=20
```
With this one, we tell the runner to retry the failed tests three times if less than 20% of all tests have failed.
```
--maxCount=4
```
Instructs the runner to execute the tests in 4 separate processes. By default, the tests are split by test count. **Keep in mind that tests in one test class will always be executed sequentially on the same thread!** If you use the **timeBased** argument, the time-distribution-based balancing will be used.
```
--testRunName="CIWebTests"
```
The name of the test run. We use it to keep the history for this particular test run and its tests. We use this history to generate the performance and ML tests failure analysis.
```
--licenseKey="122c5783-d122-4de9-85bf-ss56c5e4554"  --companyName="AutomateThePlanet" --projectName="Bellatrix"
```
When purchasing a license for the runner, you will receive a license key that you need to provide, as you need to mention your company and project names. We use them during the analysis and storing process of your results. Also, without them, we cannot notify you when the analysis report is ready.
```
--autoAnalysis
```
By default our Machine Learning auto-analysis of the test failures is turned off since it takes some time. Usually it takes less then several seconds. If you wish to turned it on use the above argument.
```
--reportPortalProject="bellatrix" --reportPortalAccessToken="asdgr435-66f5-44a3-ghjh5-7956895814ed" --reportPortalUrl="http://myreportportal.com:8080" --reportPortalLaunchName="CIWebTests"
```
The results are sent to your **[ReportPortal](https://reportportal.io/)** instance. You need to provide the name of your project, and the launch name later used to group your tests. Also, you need to provide a URL to your instance and an access token used for login. You can generate one from your **[ReportPortal](https://reportportal.io/)** profile. In the following section, you can read more about **[ReportPortal](https://reportportal.io/)**.

Additional Notes
-------
By default for BELLATRIX Web, Android, iOS, Desktop modules the runner switches automatically all app/browser behaviors to **RestartEveryTime**.
```
[App(Constants.WpfAppPath, AppBehavior.RestartEveryTime)]
[Browser(BrowserType.Chrome, BrowserBehavior.RestartEveryTime)]
```
This is needed so that all apps and browsers ran in parallel to be correctly closed. Otherwise, you risk lots of them remaining open at the end of the run. Because of this automatic behavior, if you use logic inside **TestsAct** or **TestsArrange** methods to perform operations against your app, your website, in case of parallel run, you risk experiencing unexpected errors. Preferably execute such logic inside the **TestInit** method.
```
public override void TestsArrange()
{
    _mainButton = App.ElementCreateService.CreateByName<Button>("E Button");
    _resultsLabel = App.ElementCreateService.CreateByAutomationId<Label>("ResultLabelId");
}

public override void TestsAct()
{
    _mainButton.Hover();
}
```
By default, in parallel-run mode, the BELLATRIX video recording is turned off. However, if you run your tests in Selenoid, BELLATRIX Browser/Android Instances, or cloud providers, you will have the videos as part of the special BELLATRIX Cloud Test Runner Test Failures Analysis Report.

What is ReportPortal?
-------
**[ReportPortal](http://reportportal.io/)** is a service, that provides increased capabilities to speed up results analysis and reporting through the use of built-in analytic features. ReportPortal is a great addition to the Continuous Integration and Continuous Testing process.

The tool gives you the capability to create custom dashboards. I have created a dashboard visualizing the data from our latest BELLATRIX test runs. The first one shows the passing rate of all tests. The next one displays overall statistics- how many tests were executed and various types of bugs after the investigation process. Bellow, you can find some trend charts between runs. Also, there are some nice widgets for making a comparison between the test runs duration. 

![Bellatrix](images/reportportal-configurable-dashboards.png)

There are widgets that display the latest runs flaky tests and the tests that most failed. It is a neat feature for stabilizing these tests.

![Bellatrix](images/reportportal-configurable-dashboards2.png)

In the Launches section, you can see the latest runs and filter them.

![Bellatrix](images/reportportal-all-launches.png)

When you open a failed tests it initially is set that it needs investigation after that you can mark it as Product Bug, Automation Bug or System issue like a problem in the test environment.

![Bellatrix](images/report-portal-investigation.png)

All test failure info is synced automatically and well displayed.

![Bellatrix](images/report-portal-errors-visualisation.png)

You can filter based on the bug type and check all of these tests.

![Bellatrix](images/reportportal-filters.png)