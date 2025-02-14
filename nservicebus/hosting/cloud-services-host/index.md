---
title: Hosting in Azure Cloud Services
summary: Using Azure Cloud Services to host NServiceBus.
component: CloudServicesHost
redirects:
 - nservicebus/hosting-nservicebus-in-windows-azure-cloud-services
 - nservicebus/azure/hosting-nservicebus-in-windows-azure-cloud-services
 - nservicebus/windows-azure-transport
related:
 - nservicebus/lifecycle
reviewed: 2020-04-26
---

include: cloudserviceshost-deprecated-warning

The Azure Platform and NServiceBus make a perfect fit. The Azure platform offers the scalable and flexible platform required while NServiceBus makes development on this highly distributed environment simple.

If true scale is required (e.g. in tens, hundreds, or even thousands of machines hosting each endpoint) then Cloud Services is the required deployment model.

## Cloud Services - Worker Roles

Reference the assembly that contains the Azure role entry point integration by adding a NuGet package reference to the `NServiceBus.Hosting.Azure` package to the project.

To integrate the NServiceBus generic host into the worker role entry point, create an instance of `NServiceBusRoleEntrypoint` and call its `Start` and `Stop` methods in the appropriate `RoleEntryPoint` override.

snippet: HostingInWorkerRole

Next, define the endpoint behavior. The role has been named `AsA_Worker`. Specify the transport and persistence using the `UseTransport<T>` and `UsePersistence<T>` methods.

snippet: ConfigureEndpointWithAzureHost

This will integrate and configure the default infrastructure:

 * Logs will be sent to the `Trace` infrastructure, which should have been configured with Azure diagnostic monitor trace listener by the Visual Studio tooling.

When self-hosting, everything can be configured using the API and extension methods available in the NServiceBus Azure-related packages; it's not required to reference the hosting package. To self-host an endpoint, add the required configuration to the role entry point.


## Cloud Services - Web Roles

Next to worker roles, Cloud Services also has a role type called 'Web Roles'. These are worker roles which have IIS configured, so that they run a worker role process (the entry point is in `webrole.cs`) and an IIS process in the same codebase.

Usually NServiceBus is configured as a client in the IIS process. This needs to be approached in the same way as any other website, by means of self-hosting. When self-hosting, everything can be configured using the API and the extension methods found in the NServiceBus Azure-related packages; no reference to the hosting package is required.

The configuration API is used with the following extension methods to achieve the same behavior as the `AsA_worker` role:

snippet: HostingInWebRole

A short explanation of each:

 *  Logs will be sent to the `Trace` infrastructure, which should have been configured with Azure diagnostic monitor trace listener by the Visual Studio tooling.
 * `UseTransport<AzureStorageQueueTransport>`: Sets [Azure storage queues](/transports/azure-storage-queues/) as the [transport](/transports).
 * `UsePersistence<AzureStoragePersistence>`: Configures [Azure storage](/persistence/azure-table/) for [persistence](/persistence).

## When endpoint instance starts and stops


snippet: CloudServicesStartAndStop