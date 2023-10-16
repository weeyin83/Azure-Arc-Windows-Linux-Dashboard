# Azure Arc Queries

The dashboard is built using queries from [Azure Resource Graph](https://learn.microsoft.com/azure/governance/resource-graph/overview) queries.   You can interactive with the dashboard and see the queries within the Azure Portal, but I wanted to document them here for reference. 

# Table of contents

- [Azure Arc Overview](#azure-arc-overview)
- [Windows ESU Assignment Status](#windows-esu-assignment-status)
- [Count Operating Systems](#count-operating-systems)
- [SQL Server version count](#sql-server-version-count)
- [Azure Arc Agent version](#azure-arc-agent-version)


### Azure Arc Overview

This query gives an overview of the servers that have an Arc agent installed.  It displays the name of the server, Arc agent version installed, current status of the agent, the Azure location, Azure Resource Group name and Azure subscription ID. 

It uses the information behind the resource type "microsoft.hybridcompute/machines" to gather the information.  Some of the information we need is nested within the "microsoft.hybridcompute/machines" backend JSON file, so we have to use the _extend_, it allows us to access the information. 

```bash
 resources
| where type == "microsoft.hybridcompute/machines"
| extend agentversion = properties.agentVersion
| extend state = properties.status
| extend status = case(
    state =~ 'Connected', 'Connected',
    state =~ 'Disconnected', 'Offline',
    state =~ 'Error', 'Error',
    state =~ 'Expired', 'Expired',
    '')
| project name, agentversion, status, location, resourceGroup, subscriptionId
| order by name
```

### Windows ESU Assignment Status

This query pulls out all the Windows servers that are Arc enabled and eligible to use Extended Security Updates (ESU).  It then displays the status of these servers and if they have an ESU license assigned or not. 

It uses the information behind the resource type "microsoft.hybridcompute/machines" to gather the information. You can see we've used the _where_ filtering feature to only pull out the servers that are eligible for ESU updates.  Some of the information we need is nested within the "microsoft.hybridcompute/machines" backend JSON file, so we have to use the _extend_, it allows us to access the information. 

```bash
resources
| where type=~ 'microsoft.hybridcompute/machines'
| extend esuEligibility = properties.licenseProfile.esuProfile.esuEligibility
| where esuEligibility== 'Eligible'
| extend licenseAssignmentState = properties.licenseProfile.esuProfile.licenseAssignmentState
| extend serverType = properties.licenseProfile.esuProfile.serverType
| extend osSku = properties.osSku
| extend CoreCount = toint (properties.detectedProperties.logicalCoreCount)
| project name, CoreCount, osSku, serverType, licenseAssignmentState
```

### Count Operating Systems

This query pulls out all the servers that have the Arc agent installed and then displays the operating system versions and counts them. 

```bash
 resources
| where type == "microsoft.hybridcompute/machines"
| extend osSku = properties.osSku
| project name, osSku
| summarize count() by tostring(osSku)
```

### SQL Server version count

This query looks for any servers that have the Azure Arc agent installed and have a SQL instance installed on them. It will count how many version of each SQL server version is installed.  So you will be able to see how many SQL Server 2012 servers you have, or SQL Server 2014 servers etc. 

This query uses the information behind the resource type "microsoft.azurearcdata/sqlserverinstances" to gather the information. Some of the information we need is nested within the "microsoft.azurearcdata/sqlserverinstances" backend JSON file, so we have to use the _extend_, it allows us to access the information. 

```bash
resources
| where type == 'microsoft.azurearcdata/sqlserverinstances'
| extend status = properties.status
| extend sqlversion = properties.version
| project name, sqlversion
| summarize count() by tostring(sqlversion)
```

### Azure Arc Agent version

This query looks to pull together a count of how many servers are running which Azure Arc agent version.  This helps you to determine if any servers are running older versions etc. 

This query uses the information behind the resource type "microsoft.azurearcdata/sqlserverinstances" to gather the information.

```bash
 resources
| where type == "microsoft.hybridcompute/machines"
| summarize count() by tostring(properties.agentVersion)
```