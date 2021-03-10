# QBS.NAV.ApplicationInsights

Examples of running Application Insights on older versions of NAV and Business Central

Application Insights is an incredibly powerful tool to analyse telemetry for Business Central.

In this repository you can read how to use KQL analysis on older versions of like NAV2018 or Business Central 14

![Chart1](/Images/SlowQueriesPerHour.png)
Format: ![Alt Text](url)

## Prerequisites

The examples in here run on Azure Log Analytics workspaces. This can be called the "big sister" of what is now Application Insights and you can setup a connection directly to your service tier.

If your service tier runs as a virtual machine on Azure this is as easy as a few checkmarks in the configuration. You can find information about this in the Microsoft Docs [here](https://docs.microsoft.com/en-us/azure/azure-monitor/agents/data-sources-windows-events).

