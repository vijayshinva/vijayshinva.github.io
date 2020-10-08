---
layout: post
title:  "Stopping unattended-upgrades in Azure Ubuntu VMSS"
date:   2018-11-20 12:00:00 +0530
categories: [azure]
tags: [azure]
---

## Issue
**Azure Virtual Machine Scale Sets** (VMSS) has a feature called **Automatic OS image upgrade** which allows you to automatically upgrade the virtual machine instances in the VMSS to the latest OS image. **Automatic OS image upgrades** are not always desirable and there are a lot of scenarios where you would like to manually control these upgrades.

In **Azure Resource Manager Templates** (ARM) you control this via the following setting
``` json
{ 
  "properties": { 
    "upgradePolicy": { 
      "automaticOSUpgradePolicy": { 
        "enableAutomaticOSUpgrade":  true 
      } 
    } 
  } 
}
```
Setting `enableAutomaticOSUpgrade` to *false* should, as per documentation stop the OS image from upgrading. But in our setup we found that Ubuntu virtual machine instances were automatically receiving kernel patches.

## Cause
Ubuntu (16.04 LTS and higher) has a package called **unattended-upgrades**, which is used to automatically install updated packages. **unattended-upgrades** by default is enabled and updates security patches. This means any security related kernel patches are upgraded automatically.

## Resolution
Even though setting `enableAutomaticOSUpgrade` to *false* gives you a sense that all updates on the virtual machine will be turned off, it is not correct. `enableAutomaticOSUpgrade` will only control if Azure pushes updates to the virtual machines. The base OS on the virtual machine is not affected by this setting.

To completely turn off **unattended-upgrades** in Ubuntu, you can either edit **/etc/apt/apt.conf.d/50unattended-upgrades**
``` 
    Unattended-Upgrade::Allowed-Origins {
            "${distro_id}:${distro_codename}";
            "${distro_id}:${distro_codename}-security";
    //      "${distro_id}:${distro_codename}-updates";
    //      "${distro_id}:${distro_codename}-proposed";
    //      "${distro_id}:${distro_codename}-backports";
    };
```
and comment out the line **"${distro_id}:${distro_codename}-security";** 

or edit **/etc/apt/apt.conf.d/20auto-upgrades** 
```
    APT::Periodic::Update-Package-Lists "1";
    APT::Periodic::Download-Upgradeable-Packages "1";
    APT::Periodic::AutocleanInterval "7";
    APT::Periodic::Unattended-Upgrade "1";
```
and set **APT::Periodic::Unattended-Upgrade** to **"0"**;

You can automate this change by using the **Custom Script Extension** in ARM to run a bash script which updates these settings.

####  References

[Azure virtual machine scale set automatic OS image upgrades](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade)

[Ubuntu unattended-upgrades](https://help.ubuntu.com/lts/serverguide/automatic-updates.html.en)
