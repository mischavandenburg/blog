---
title: "Pipelines: Continuous Monitoring"
date: 2023-01-03T08:30:03+01:00
tags:
- Azure

- Explanations
---

This term can be confusing. Initially I thought it meant monitoring of the pipelines themselves. However, in the context of Azure Release Pipelines, continuous monitoring refers to something else.

Continuous monitoring leverages metrics from other services such as Application Insights. You can set up release gates based on these metrics. For example, you can set up a release gate to roll back the deployment if an alert is being fired for high CPU usage in the application.

You can set up several of these checks. If all these checks pass, the pipeline can proceed.
