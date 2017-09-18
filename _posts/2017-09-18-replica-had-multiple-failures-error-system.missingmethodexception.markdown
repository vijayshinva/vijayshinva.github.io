---
layout: post
title:  "Azure Service Fabric : Replica had multiple failures ... Error = System.MissingMethodException (-2146233069)"
date:   2017-09-18 12:00:00 +0530
categories: [azure, service-fabric]
tags: [azure, service-fabric]
---

When you deploy a **Service Fabric Application** to a **Service Fabric Cluster**, the application fails and ends up with Health State as `Error`. A health state of `Error` means the Service Fabric Cluster could not successfully spawn up the necessary number of service instances. One reason why this could happen is because of **Microsoft Azure Service Fabric SDK** version mismatch. In such cases you will see error messages with exception `System.MissingMethodException`

My machine had **Microsoft Azure Service Fabric SDK v2.6.220** installed.

![Microsoft Azure Service Fabric SDK v2.6.220](/img/posts/post-2017-09-18-webpi.png)

Whenever I created a new Service Fabric project on my machine, **Visual Studio** would add references to the __Microsoft.ServiceFabric.*__ NuGet packages for the SDK version v2.6.220.

The NuGet packages that you add to your **Service Fabric Visual Studio Solution** should match the **Microsoft Azure Service Fabric SDK** version installed on the machine. 

As Microsoft releases newer versions of the SDK new versions of these packages are made available in the NuGet gallery. As a result the **Nuget Manager** in Visual Studio will also start showing you updates for the __Microsoft.ServiceFabric.*__ packages.

![Nuget Manager](/img/posts/post-2017-09-18-nuget-update.png)

The **NuGet Manager** has no way to validate the version of **Microsoft Azure Service Fabric SDK** installed on the system and shows all the available versions from the NuGet.org gallery.

If you update the NuGet packages to the latest version, there is a mismatch between the NuGet packages your **Visual Studio Solution** is using and the underlying **Service Fabric Runtime**. Your application will build fine but will start failing when it is deployed with errors like the ones below.

![ASP.NET Core Error](/img/posts/post-2017-09-18-asp-error.png)

```
Unhealthy event: SourceId='System.RA', Property='ReplicaOpenStatus', HealthState='Warning', ConsiderWarningAsError=false.
Replica had multiple failures in_Node_3 API call: IStatelessServiceInstance.Open(); Error = System.MissingMethodException (-2146233069)
Method not found: 'System.String System.Fabric.ServiceContext.get_PublishAddress()'.
   at Microsoft.ServiceFabric.Services.Communication.AspNetCore.AspNetCoreCommunicationListener.OpenAsync(CancellationToken cancellationToken)
   at Microsoft.ServiceFabric.Services.Runtime.StatelessServiceInstanceAdapter.d__20.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Services.Runtime.StatelessServiceInstanceAdapter.d__14.MoveNext()
```

![Actor Service Error](/img/posts/post-2017-09-18-actor-error.png)

```
Unhealthy event: SourceId='System.RA', Property='ReplicaOpenStatus', HealthState='Warning', ConsiderWarningAsError=false.
Replica had multiple failures in_Node_1 API call: IStatefulServiceReplica.ChangeRole(P); Error = System.MissingMethodException (-2146233069)
Method not found: 'System.String System.Fabric.ServiceContext.get_ListenAddress()'.
   at Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime.FabricTransportServiceRemotingListener..ctor(ServiceContext serviceContext, IServiceRemotingMessageHandler messageHandler, FabricTransportRemotingListenerSettings listenerSettings)
   at Microsoft.ServiceFabric.Actors.Remoting.FabricTransport.FabricTransportActorRemotingProviderAttribute.CreateServiceRemotingListener(ActorService actorService)
   at Microsoft.ServiceFabric.Services.Runtime.StatefulServiceReplicaAdapter.d__27.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Services.Runtime.StatefulServiceReplicaAdapter.d__19.MoveNext()
```
In this case, the error messages indicate that the **Service Fabric Runtime** could not find certain Methods which the NuGet package assemblies where trying to call, but the errors could change. You can see these error messages in the **Service Fabric Cluster Manager**

![Service Fabric Manager](/img/posts/post-2017-09-18-sf-manager.png)

To avoid these errors you will either have to update the **Microsoft Azure Service Fabric SDK** installed on the machine to the latest version or downgrade the NuGet packages that you referred in the **Visual Studio Solution**. 

And always remember, the versions of the **Microsoft Azure Service Fabric SDK** and the __Microsoft.ServiceFabric.*__ NuGet packages should be in sync.