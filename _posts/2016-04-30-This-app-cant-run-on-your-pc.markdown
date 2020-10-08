---
layout: postold
title:  "This app can't run on your PC"
date:   2016-04-30 12:00:00 +0530
categories: [windows]
tags: [windows]
---

You are no longer able to run your application on Windows and get the following error message.

{% highlight windows %}
This app can't run on your PC
To find a version for your PC, check with the software publisher.
{% endhighlight %}


![This app can't run on your PC](/img/posts/this-app-cant-run.png)


## Cause
Windows reads the first few bytes of your program executable(exe) to determine if it can run it. These bytes are called the PE header. If for some reason Windows cannot interpret the PE header or finds that it is incompatible, it will show the above error message.

## Resolution
Well resolution depends on the situation ....

In my case, I had a machine that used to run msbuild.exe regularly. And one fine morning, I started getting this error whenever I tried to run msbuild.exe. Few hours of investigation and I found that msbuild.exe was now a 0KB file.

![This app can't run on your PC](/img/posts/this-app-cant-run-msbuild.png)

So either the AntiVirus software on my machine or a malware had tampered that executable file.



