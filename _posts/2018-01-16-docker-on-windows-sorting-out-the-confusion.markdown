---
layout: post
title:  "Docker on Windows - Sorting out the confusion"
date:   2018-01-16 12:00:00 +0530
categories: [docker]
tags: [docker]
---

I heard we can now run Docker containers on Windows but 

- Are Windows containers and Linux containers the same?
- Can I run Linux containers on Windows?
- Can I run Windows containers on Linux?
- Can I only run .NET Core apps on Windows containers?
- Can I run .NET (full framework like v4.6.2) apps on Windows containers?
- Can I run containerized Windows apps on Linux?
- Can I have a docker cluster of Linux and Windows together?
- What is Docker for Windows?
- What was Docker Toolbox for Windows?
- Windows Nano or Windows Server Core?

If you are bothered by any of these questions read ahead...

**Docker** started off on **Linux**. It used the features which the **Linux kernel** provided (namespaces, cgroups etc.) to create an 'application isolation' system. Until then if you wanted to isolate your applications - **Virtual Machines** were the primary way. **Docker** caught on and we soon had supporting software like **Kubernetes**, **Docker Swarm**, **Docker Compose** etc.

One such supporting software was [**Docker Tookbox for Windows**](https://docs.docker.com/toolbox/toolbox_install_windows/). People having Windows computers could now work on **Linux containers**. The intent was to give developers, having Windows machines a way to work with **Docker** and **Linux containers**. It achieved this by installing [**Oracle VM VirtualBox**](https://www.virtualbox.org/) and creating a **Linux VM** on it. The [**docker cli**](https://docs.docker.com/engine/reference/commandline/cli/) was installed on the Windows machine but the commands were forwarded to the **Linux VM** where the docker daemon was running. So, you were essentially working with **Linux containers** in a **Linux VM** on **Oracle VM VirtualBox**. In fact you could just install **Oracle VM VirtualBox** or **Hyper-V** on a Windows machine, create a **Linux VM**, **ssh** to it and install **docker** on it.

Then [Microsoft](https://www.docker.com/microsoft) jumped on the containerization bandwagon. First it made changes to its **Windows kernel** to support application isolation (**Windows Server Containers**) and second along with Docker Inc. it released [**Docker for Windows**](https://docs.docker.com/docker-for-windows/)

Similar to **Linux containers**, today you can have **Windows containers**. But that does not mean these are interoperable. **Linux containers** have their base as **Linux** and will run on **Linux container hosts**. **Windows containers** have their base as **Windows** and will run on **Windows container hosts**.

Many developers and administrators for some reason think Microsoft has added support to run **Linux containers** on **Windows**. And the confusion is usually based on a loose reading of the blog post [Preview: Linux containers on Windows](https://blog.docker.com/2017/09/preview-linux-containers-on-windows/) and a totally unrelated event [Run Bash on Ubuntu on Windows](https://blogs.windows.com/buildingapps/2016/03/30/run-bash-on-ubuntu-on-windows/)

**Docker for Windows** allows developers to work on both Windows and Linux containers on a Windows machine. When in **Windows container mode** it uses the inbuilt container support from the **Windows kernel**. When in **Linux container mode** it uses a **Linux** VM on **Hyper-V** (similar to what **Docker Toolbox for Windows** did) to host the containers.

**The next question then is what can you run on Windows containers?**

A **Docker** container image is composed of a series of layers. On Linux, the **base os layer** is usually a very minimalistic linux distro (alpine linux is just 5 MB). So for the Windows containers, Microsoft launched the [**Windows Server Nano Server**](https://docs.microsoft.com/en-us/windows-server/get-started/getting-started-with-nano-server) which can act as the [**base os image**](https://hub.docker.com/r/microsoft/nanoserver/) (currently 1.1 GB). For .NET developers, the big catch is that it only supports [**.NET Core**](https://docs.microsoft.com/en-us/dotnet/core/) and does not support the full [**.NET Framework**](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server). Native developers can use the [**NanoServerApiScan tool**](https://blogs.technet.microsoft.com/nanoserver/2016/04/27/nanoserverapiscan-exe-updated-for-tp5/) to check if their applications can run on **Windows Nano Server**.
**Windows containers** can also run [**Windows Server Core**](https://msdn.microsoft.com/en-us/library/ee391626(v=vs.85).aspx) as their [base os image](https://hub.docker.com/r/microsoft/windowsservercore/) (currently 10.4 GB). If you use **microsoft/windowsservercore** as your base image you can then run the full **.NET framework** along with all **Win32** applications that Windows Server supports. Your **Windows Containers** can have either **microsoft/nanoserver**  or **microsoft/windowsservercore** as the **base os layer**. 

- **Are Windows containers and Linux containers the same?**

    No. **Linux containers** use the features provided by the **Linux kernel** to achieve application isolation and need **Linux** host machines to run them. **Windows containers** use the features provided by the **Windows kernel** to achieve application isolation and need **Windows** host machines to run them. 
- **Can I run Linux containers on Windows?**

    As a developer yes. In production no. **Docker for Windows** allows developers with Windows machines to run **Linux containers** on a **Windows** host machine (it uses a Linux VM on **Hyper-V** to host the actual **Linux container**). But orchestrators like **Kubernetes** or **Docker Swarm** are not aware of this feature.
- **Can I run Windows containers on Linux?**

    No.
- **Can I only run .NET Core apps on Windows containers?**

    If your container base image is **microsoft/nanoserver**, it only supports **.NET Core**. But you can also use **microsoft/windowsservercore** as your base os image.
- **Can I run .NET (full framework like v4.6.2) apps on Windows containers?**

    Yes you can with **microsoft/windowsservercore** as your base os image.
- **Can I run containerized Windows apps on Linux?**

    **.NET Core** applications can run natively on **Linux**. So a **Linux container** built with .NET Core apps can run on a **Linux** host machine. But you cannot run **Win32** applications or **.NET framework** applications on **Linux containers**. Also **Windows containers** cannot be run on **Linux** host machines. 
- **Can I have a docker cluster of Linux and Windows together?**

    Yes. **Docker Swarm** has built in support for running [**Linux+Windows mixed OS clusters**](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/swarm-mode). Similarly **Kubernetes** is building support for [**Windows Server Containers**](https://kubernetes.io/docs/getting-started-guides/windows/)
- **What is Docker for Windows?**

    **Docker for Windows** is a development environment where, on a Windows developer host machine you can run **Windows** or **Linux containers**. For **Linux containers** a **Linux** VM running on **Hyper-V** is used as the host.
- **What was Docker Toolbox for Windows?**

    Before **Docker for Windows**, Docker Inc had released **Docker Toolbox for Windows** which allowed developers to work with **Linux containers** on a Windows host machine. It used a **Linux** VM on **Oracle VM VirtualBox** to host the actual **Linux container**.
- **Windows Nano or Windows core?**

    If you are starting a new project today, you should target **microsoft/nanoserver**. For legacy applications use **microsoft/windowsservercore**. Certain SDKs / Frameworks might force you to use **microsoft/windowsservercore** due to compatibility reasons.

Apart from the **base os images**, Microsoft also has a list of official images like **microsoft/dotnet-framework**, **microsoft/wcf** etc, that can be used to containerize specific applications. The full list is at [https://hub.docker.com/u/microsoft](https://hub.docker.com/u/microsoft).

**What about hosting a production-ready docker cluster ?**

If you are using **Microsoft Azure** as your cloud provider you have 3 ways to run a Docker cluster.
- Manually create the cluster using a bunch of networked VMs in **Azure**
    - Manual administration effort.
    - You can use either **Docker Swarm** or **Kubernetes** as the orchestrator and create a cluster
    - You get support for hybrid os clusters as provided by the orchestrator
- Azure Container Service ([ACS](https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-intro-kubernetes))
    - Cluster creation is managed.
    - You get **Docker Swarm**, **Kubernetes** and a few other options as the orchestrator
    - You get support for hybrid os clusters only using [**acs-engine**](https://github.com/Azure/acs-engine/blob/master/examples/windows/kubernetes-hybrid.json)
    - Is GA
- Azure Containers Service Managed ([AKS](https://azure.microsoft.com/en-in/services/container-service/))
    - Cluster creation/administration is managed.
    - You get **Kubernetes** as the orchestrator
    - Is in BETA, and hybrid os clusters support is still missing.

**Why AKS when you had ACS ?** 

Well **AKS** is a truely managed kubernetes offering from **Azure** (K8S as a Service). In **ACS** your typical minimal cluster would have 3 master nodes and 3 worker agents. In **AKS** that cluster would only have 3 worker agents (the stuff the master nodes used to do, is now managed by **Azure** and for free).