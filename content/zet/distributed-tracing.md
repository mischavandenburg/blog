---
title: "Distributed Tracing"
date: 2023-01-03T07:58:44+01:00
tags:

- Explanations
- Azure
---
Debugging is done using [call stacks](/zet/call-stacks/) in monolithic applications. Nowadays it is more common to deploy an application using a microservices architecture. Microservices make it easier to update certain parts of the application, and allow for more frequent deployments.

Using microservices does have a disadvantage: you cannot use the local call stack for debugging, because calls are sent to different microservices. 

Distributed tracing is an implementation of the call stack in the cloud. It is usually implemented by adding an agent, [SDK](/zet/sdk/), or library to the service. In Azure you can enable distributed tracing via Application Insights through auto-instrumentation or SDKs.

[Unified cross-component transaction diagnostics](https://learn.microsoft.com/en-us/azure/azure-monitor/app/transaction-diagnostics)
