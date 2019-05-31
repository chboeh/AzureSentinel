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

![Log searches](pictures/queries-overview.png)

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

![Orgnization-KQL](pictures/queries-tables.png)


## Reference

- [Query language reference](/azure/kusto/query)  is the complete language reference for the Kusto query language.
- [Azure Monitor log query language differences](data-explorer-difference.md) describes differences between versions of the Kusto query language.
- [Standard properties in Azure Monitor log records](../../azure-monitor/platform/log-standard-properties.md) describes properties that are standard to all Azure Monitor log data.
- [Perform cross-resource log queries in Azure Monitor](../../azure-monitor/log-query/cross-workspace-query.md) describes how to write log queries that use data from multiple Log Analytics workspaces and Application Insights applications.

## Examples

- [Azure Monitor log query examples](examples.md) provides example queries using Azure Monitor log data.

## Lessons

- [Working with strings in Azure Monitor log queries](string-operations.md) describes how to work with string data.
- [Working with date time values in Azure Monitor log queries](datetime-operations.md) describes how to work with date and time data. 
- [Aggregations in Azure Monitor log queries](aggregations.md) and [Advanced aggregations in Azure Monitor log queries](advanced-aggregations.md) describe how to aggregate and summarize data.
- [Joins in Azure Monitor log queries](joins.md) describes how to join data from multiple tables.
- [Working with JSON and data Structures in Azure Monitor log queries](json-data-structures.md) describes how to parse json data.
- [Writing advanced log queries in Azure Monitor](advanced-query-writing.md) describes strategies for creating complex queries and reusing code.
- [Creating charts and diagrams from Azure Monitor log queries](charts.md) describes how to visualize data from a log query.

## Cheatsheets

-  [SQL to Azure Monitor log query](sql-cheatsheet.md) assists users who are already familiar with SQL.






