---
layout: post
title:  "Visual Studio 2015 Enterprise with Update 1 - Could not load file or assembly Microsoft.VisualStudio.ConnectedServices, Version=2.0.0.0"
date:   2016-03-31 12:00:00 +0530
categories: [visual studio, .net]
tags: [visual studio, .net]
---

On a fresh installation of Visual Studio 2015 Enterprise with Update 1 you might get the following error

{% highlight bamboo %}
Could not load file or assembly 
'Microsoft.VisualStudio.ConnectedServices, Version=2.0.0.0,
Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its
dependencies. The system cannot find the file specified.
{% endhighlight %}


![Visual Studio Error Message](/img/posts/Microsoft.VisualStudio.ConnectedServices.Error.png)

You will not be able to close Visual Studio as the above popup window will continiously appear. Also Visual Studio Repair will not fix this issue.

## Cause
Visual Studio (devenv.exe) cannot locate the ```Microsoft.VisualStudio.ConnectedServices.dll``` assembly, which is used by other assemblies such as Microsoft.VisualStudio.ConnectedServices.Framework.dll. This assembly is part of the Visual Studio Connected Services SDK.

## Resolution
1. Create a Console Application
2. Refer to the Visual Studio Connected Services SDK Nuget Package https://www.nuget.org/packages/Microsoft.VisualStudio.ConnectedServices/
3. Copy the Microsoft.VisualStudio.ConnectedServices.dll assembly that gets added to your packages/Microsoft.VisualStudio.ConnectedServices.2.0.0/lib/net45 folder to the folder where devenv.exe is located, usually C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE
4. Kill devenv.exe and start Visual Studio again.
