---
layout: post
title:  "New-NetNat : The parameter is incorrect."
date:   2017-04-27 12:00:00 +0530
categories: [windows, hyper-v]
tags: [windows, hyper-v]
---

Hyper-V on Windows 10 now supports Network Address Translation (NAT). The steps to set it up are documented in this MSDN article [Setup NAT Network][setup-nat-network]

But while running the `New-NetNat` command like the one below,

{% highlight powershell %}
New-NetNat -Name MyNATnetwork -InternalIPInterfaceAddressPrefix 192.168.0.0/24
{% endhighlight %}

You might receive the following error

{% highlight powershell %}
New-NetNat : The parameter is incorrect.
{% endhighlight %}

![New-NetNat Error Message](/img/posts/New-NetNat-Error.png)

The error is misleading as it points to the parameter being passed.

On my machine the error was because of an existing NetNat entry. The way you figure this out is by running the command `Get-NetNat`. This will list any existing NetNat entries. You can remove the old entry using the command `Remove-NetNat`

[setup-nat-network]: https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/setup-nat-network
