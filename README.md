# Azure Arc Windows/Linux Dashboard

This dashboard has been created to enable you to view several elements of the Windows or Linux servers you have deployed the Azure Arc agent to in one simple view.   Within this dashboard you will be able to see: 
* The Windows/Linux servers that have the Arc agent installed
* A count of what operating systems are being used by these Arc enabled servers
* A view of what Arc agent version is installed
* Current Windows ESU assignment status

![arc-dashboard-example](https://github.com/weeyin83/Azure-Arc-Windows-Linux-Dashboard/assets/13692824/917cab22-455b-4c6f-a3e6-3bd32d54e021)


# How to Enable Azure Dashboard for Arc Windows/Linux
This article will show you how to use the [Azure-Arc-Windows-Linux-Dashboard.json](Azure-Arc-Windows-Linux-Dashboard.json) file to create a custom dashboard in the [Azure portal](https://learn.microsoft.com/azure/azure-portal/azure-portal-dashboards).



### Steps to Follow

1. Log in to [Azure Portal](https://portal.azure.com/)
2. Click on **Dashboard** from the Azure Portal menu. You may already see the dashboard view by default.
![](https://learn.microsoft.com/azure/azure-portal/media/azure-portal-dashboards/portal-menu-dashboard.png)
3. Click on **Create**
4. Select **Custom** Dashboard
5. You will be prompted to customise the new dashboard, click on **cancel**
6. Now select **Upload** and upload the [Azure-Arc-Windows-Linux-Dashboard.json](Azure-Arc-Windows-Linux-Dashboard.json) file
7. If you want to edit the dashboard, please refer to this [link](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards#edit-a-dashboard).

<a name=disclaimers></a>

## Disclaimers
The code included in this sample is not intended to be a set of best practices on how to build scalable enterprise grade applications. This is beyond the scope of this quick start sample.
