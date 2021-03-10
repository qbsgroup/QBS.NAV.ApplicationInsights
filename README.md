# QBS.NAV.ApplicationInsights

Examples of running Application Insights on older versions of NAV and Business Central

Application Insights is an incredibly powerful tool to analyse telemetry for Business Central.

In this repository you can read how to use KQL analysis on older versions of like NAV2018 or Business Central 14

![Chart1](/Images/SlowQueriesPerHour.png)
Format: ![Alt Text](url)

## Prerequisites

The examples in here run on Azure Log Analytics workspaces. This can be called the "big sister" of what is now Application Insights and you can setup a connection directly to your service tier.

### Azure VM

If your service tier runs as a virtual machine on Azure this is as easy as a few checkmarks in the configuration. You can find information about this in the Microsoft Docs [here](https://docs.microsoft.com/en-us/azure/azure-monitor/agents/data-sources-windows-events).

### Physical Machine

This means anything non-Azure, so VMWare, HyperV or even hosted by Amazon.

In this case you need to install a tool called the MMA Agent. This will forward data from the Windows Event Log to Azure Log Analytics. Documentation can be found [here](https://docs.microsoft.com/en-us/azure/azure-monitor/agents/agent-windows).

## The KQL

Data in the Log is stored in different containers with their own columns. The container we will use is called **Event**.

Each row in the Windows Event log gets a row in the Event table with a timestamp. The information from Dynamics NAV or Business Central is stored in one big field called **ParameterXML**.

### Regular Expressions

In order to get meaningful information we need to use regular expressions on the **ParameterXML** column such as this example:

`object = strcat(extract("AppObjectType:\\s{1,}([^\\ ]+)\\s", 1, ParameterXml), extract("AppObjectId:\\s{1,}([^\\ ]+)\\s", 1, ParameterXml))`

This reads the object type and id from the Parameter XML.
