# QBS.NAV.ApplicationInsights

Examples of running Application Insights on older versions of NAV and Business Central

Application Insights is an incredibly powerful tool to analyse telemetry for Business Central.

In this repository you can read how to use KQL analysis on older versions of like NAV2018 or Business Central 14

![Chart1](/Images/SlowQueriesPerHour.png)

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

#### Object Type

`object = strcat(extract("AppObjectType:\\s{1,}([^\\ ]+)\\s", 1, ParameterXml), extract("AppObjectId:\\s{1,}([^\\ ]+)\\s", 1, ParameterXml))`

#### Execution Time
`executionTime = toint(executionTime = extract("Execution time:\\s{1,}([^\\ ]+)\\s", 1, ParameterXml))`

#### SQL Query
`query = strcat(extract("SELECT\\s.*FROM\\s.*WHERE\\s.*", 0, ParameterXml), extract("DELETE\\s.*FROM\\s.*WHERE\\s.*", 0, ParameterXml), extract("UPDATE\\s.*SET\\s.*WHERE\\s.*", 0, ParameterXml), extract("BeginTransaction\\s.*", 0, ParameterXml), extract("Commit\\s.*", 0, ParameterXml), extract("Rollback\\s.*", 0, ParameterXml), extract("INSERT\\s.*VALUES\\s.*", 0, ParameterXml), extract("SELECT\\s.*FROM\\s.*", 0, ParameterXml), extract("DECLARE\\s.*INSERT\\s.*", 0, ParameterXml))`

## The Caveat

This is a fantastic alternative approach for performance troubleshooting, finding frequent error messages, dangling job queue processes and failed print jobs.

Every Message and Error can be written to the event log and thus the Azure Log. The standard treshold for query duration is 1 second. This is a good start. After a while if your system is running smooth you can start logging more queries even downto 50ms on the Job Queue or 250ms on a UI tier.

Be careful not to blow up your service tier!

# The Result

You can see exactly which object is causing issues

![Details](/Images/SlowQueriesDetails.png)

And even get the full stack trace

![Stacktrace](/Images/StackTrace.png)

# Learn More
QBS Group offers guided workshops and PTS services around Application Insights. More information can be found [here](https://www.qbsgroup.com/qbstraining/application-insights-for-business-central-2/).