# Understanding Kusto Query Language

Kusto is a service for storing and running interactive analytics over Big Data.

It is based on relational database management systems, supporting entities such as databases, tables, and columns, as well as providing complex analytics query operators (such as calculated columns, searching and filtering or rows, group by-aggregates, joins).

Kusto offers excellent data ingestion and query performance by "sacrificing" the ability to perform in-place updates of individual rows and cross-table constraints/transactions. Therefore, it supplants, rather than replaces, traditional [RDBMS](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-technical-overview) systems for scenarios such as [OLTP](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/online-transaction-processing
) and data warehousing.

As a Big Data service, Kusto handles structured, semi-structured (e.g. JSON-like nested types), and unstructured (free-text) data equally well.

## Interacting with Kusto

The main way for users to interact with Kusto is by using one of the many client tools available for Kusto. While SQL queries to Kusto are supported, the primary means of interaction with Kusto is through the use of the Kusto query language to send data queries, and through the use of control commands to manage Kusto entities, discover metadata, etc. Both queries and control commands are basically short textual "programs".

### Queries

A Kusto query is a read-only request to process Kusto data and return the results of this processing, without modifying the Kusto data or metadata. Kusto queries can use the SQL language, or the Kusto query language. As an example for the latter, the following query counts how many rows in the Logs table have the value of the Level column equals the string Critical:
Kusto

```
Logs
| where Level == "Critical"
| count
```
Queries cannot start with the dot (.) character or the hash (#) character.

### Where log queries are used

The different ways that you will use queries in Azure Monitor include the following:

- **Portal.** You can perform interactive analysis of log data in the [Azure portal](portals.md).  This allows you to edit your query and analyze the results in a variety of formats and visualizations.  
- **Alert rules.** [Alert rules](../platform/alerts-overview.md) proactively identify issues from data in your workspace.  Each alert rule is based on a log search that is automatically run at regular intervals.  The results are inspected to determine if an alert should be created.
- **Dashboards.** You can pin the results of any query into an [Azure dashboard](../learn/tutorial-logs-dashboards.md) which allow you to visualize log and metric data together and optionally share with other Azure users. 
- **Views.**  You can create visualizations of data to be included in user dashboards with [View Designer](../platform/view-designer.md).  Log queries provide the data used by [tiles](../platform/view-designer-tiles.md) and [visualization parts](../platform/view-designer-parts.md) in each view.  

- **Export.**  When you import log data from Azure Monitor into Excel or [Power BI](../platform/powerbi.md), you create a log query to define the data to export.
- **PowerShell.** You can run a PowerShell script from a command line or an Azure Automation runbook that uses [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) to retrieve log data from Azure Monitor.  This cmdlet requires a query to determine the data to retrieve.
- **Azure Monitor Logs API.**  The [Azure Monitor Logs API](../platform/alerts-overview.md) allows any REST API client to retrieve log data from the workspace.  The API request includes a query that is run against Azure Monitor to determine the data to retrieve.

![Log searches](https://github.com/chboeh/MsftEntropy/blob/master/Training/Pictures/queries-overview.png)

### Control Commands 
Control commands are requests to Kusto to process and potentially modify data or metadata. For example, the following control command creates a new Kusto table with two columns, Level and Text:
Kusto

```
.create table Logs (Level:string, Text:string)
```
Control commands have their own syntax (which is not part of the Kusto query language syntax, although the two share many concepts). In particular, control commands are distinguished from queries by having the first character in the text of the command be the dot (.) character (which cannot start a query). This distinction prevents many kinds of security attacks, simply because this prevents embedding control commands inside queries.

Not all control commands modify Kusto data or metadata. A large class of commands, the commands that start with .show, are used to display metadata or data about Kusto. For example, the .show tables command returns a list of all tables in the current database.

## How is Log Data Orgnized in Log Anayltics workspace?

When you build a query, you start by determining which tables have the data that you're looking for. Different kinds of data are separated into dedicated tables in each Log Analytics workspace. Documentation for different data sources includes the name of the data type that it creates and a description of each of its properties. Many queries will only require data from a single table, but others may use a variety of options to include data from multiple tables.

While Application Insights stores application data such as requests, exceptions, traces, and usage in Azure Monitor logs, this data is stored in a different partition than the other log data. You use the same query language to access this data but must use the Application Insights console or Application Insights REST API to access it. You can use cross-resources queries to combine Application Insights data with other log data in Azure Monitor.

![Orgnization-KQL](https://github.com/chboeh/MsftEntropy/blob/master/Training/Pictures/queries-tables.png)








