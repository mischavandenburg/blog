---
title: "Application Insights: Telemetry Sampling"
date: 2023-01-03T08:41:09+01:00
tags:
- Azure
- Zettelkasten
- Explanations
---
Telemetry is the collection of measurements or other data at remote points, and transmitting that data to a receiver for monitoring. 

Sampling is used to reduce telemetry traffic and costs for storage and data in Application Insights.

For small and medium sized applications sampling is generally not necessary. 

Advantages of sampling:
- Throttling data when the application suddenly sends a high volume of telemetry in a short time
    - This saves costs!
- Keeping a pricing tier quota
- Reduce network traffic from telemetry collection

Three different kinds of sampling:
- adaptive sampling
    - automatically adjusts volume of telemetry
    - from ASP.NET or Azure Functions
    - only for these two
- fixed-rate sampling
    - rate is set by the administrator
    - use when you have a clear idea of the appropriate sampling percentage
    - reduces volume from 
        - ASP.NET or ASP.NET Core server
        - Java server
        - Python applications
        - User browsers
- ingestion sampling
    - used when monthly quota is often met
    - reduces amount of processed and retained traffic by Application Insights
        - less processing = less cost
        - doesn't reduce telemetry traffic sent from the app
        - happens at Applications Insight service endpoint
    - disabled if SDK samples telemetry
    - can set sampling rate without redeploying the app
    - only applies when no other sampling is in effect
        - supports all Application Insights SDK's
