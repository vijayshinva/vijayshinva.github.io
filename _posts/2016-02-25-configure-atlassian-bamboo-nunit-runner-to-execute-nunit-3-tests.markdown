---
layout: post
title:  "Configure Atlassian Bamboo(v5.9.4) NUnit Runner to execute NUnit 3 Tests"
date:   2016-02-25 12:00:00 +0530
categories: [devops, bamboo, nunit, tdd, ci]
tags: [devops, bamboo, nunit, tdd, ci]
---
Atlassian Bamboo(v5.9.4 build 5919) ships with a NUnit Runner Build Task. It can execute NUnit tests and parse the generated results to infer the number of tests that have passed, failed and skipped. But when you try to use this Build Task with NUnit 3 you will face the following issues.

##Issue 1
If you configure NUnit Runner Build Task to run NUnit 3 Tests, you will get the following error message 

```
Invalid argument: -xml=<TestResult.xml>
```

####Cause
NUnit Runner Build Task on Atlassian Bamboo(v5.9.4 build 5919) is designed to work with NUnit 2. The NUnit Runner Build Task translates your configuration to a command 

{% highlight bamboo %}
<Executable> <NUnit Test Files> -xml=<Result Filename> <Command Line Options>

e.g. nunit3-console.exe MyNUnitProject.nunit -xml=TestResult.xml --config=Debug
{% endhighlight %}

The -xml argument is no longer valid with NUnit 3 (nunit3-console.exe). Instead NUnit 3 now uses the \-\-result argument.

##Issue 2
If you configure NUnit Runner Build Task to run NUnit 3 Tests, the results infered are misconstrued. 

For example, the below images show how the Bamboo Build Log shows 31 tests passed but the Bamboo UI misinterprets the results and shows 31 tests as Quarantined/skipped

![NUnit3 Console Output](/img/posts/nunit-console-op.png)
![Bamboo NUnit 3 Output Inference](/img/posts/bamboo-inference.png)

####Cause
NUnit 3 uses a different file format to store the results as compared to NUnit 2. NUnit Runner Build Task on Atlassian Bamboo(v5.9.4 build 5919) expects the NUnit results in NUnit 2 format. 


##Resolution

For the first issue, we have to replace the -xml argument with \-\-result. This can be achieved using argument manipulation in a batch file. For the second issue we need to append the result file name with ```;format=nunit2```. Unfortunately the Bamboo UI will not allow you to set that, as its validation logic fails if the filename does not end with .xml.

####Step 1
Instead of configuring the NUnit Runner Build Task's Executable as ```nunit3-console.exe```, configure it to a batch file called ```nunit-console.bat```.

####Step 2
Edit the nunit-console.bat file and add the code below

{% highlight bat %}
@echo off 
SET projectvar=%1
SET xmlvar=%2
CALL SET xmlvar=%%xmlvar:-xml=--result%%
SET outputvar=%3;format=nunit2
SHIFT
SHIFT
SHIFT
SET remvar=%1
:loop
SHIFT
if [%1]==[] GOTO afterloop
SET remvar=%remvar% %1
GOTO loop
:afterloop
REM Ensure PATH includes nunit3-console.exe or edit the line below to include full path.
nunit3-console.exe %projectvar% %xmlvar% %outputvar% %remvar%
{% endhighlight %}

The batch file above translates 

{% highlight bamboo %}
<Executable> <NUnit Test Files> -xml=<Result Filename> <Command Line Options>
e.g. nunit3-console.bat MyNUnitProject.nunit -xml=TestResult.xml --config=Debug
{% endhighlight %}

to

{% highlight bamboo %}
<Executable> <NUnit Test Files> --result=<Result Filename>;format=nunit2 <Command Line Options>
e.g. nunit3-console.exe MyNUnitProject.nunit --result=TestResult.xml;format=nunit2 --config=Debug
{% endhighlight %}

This will trigger the nunit3.console.exe with the proper \-\-result argument and generate the output in NUnit 2 format. The NUnit Runner Build Task should now be able to execute NUnit 3 tests and parse the results properly.
