# Azure Arc Queries

The dashboard is built using queries from Azure Resource Graph queries.   You can interactive with the dashboard and see the queries within the Azure Portal, but I wanted to document them here for reference. 

# Table of contents

- [Azure Arc Overview - Overview of the Arc agents installed and current status](#lab-deployment-in-azure)



### Azure Arc Overview - Overview of the Arc agents installed and current status

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