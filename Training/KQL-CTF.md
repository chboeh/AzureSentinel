# Understanding Kusto Query Language

A Kusto query is a read-only request to process data and return results. The request is stated in plain text, using a data-flow model designed to make the syntax easy to read, author, and automate. The query uses schema entities that are organized in a hierarchy similar to SQL's: databases, tables, and columns.
The query consists of a sequence of query statements, delimited by a semicolon (;), with at least one statement being a tabular expression statement which is a statement that produces data arranged in a table-like mesh of columns and rows. The query's tabular expression statements produce the results of the query.
The syntax of the tabular expression statement has tabular data flow from one tabular query operator to another, starting with data source (e.g. a table in a database, or an operator that produces data) and then flowing through a set of data transformation operators that are bound together through the use of the pipe (|) delimiter.

## How is Log Data Orgnized in Log Anayltics workspace?

When you build a query, you start by determining which tables have the data that you're looking for. Different kinds of data are separated into dedicated tables in each Log Analytics workspace. Documentation for different data sources includes the name of the data type that it creates and a description of each of its properties. Many queries will only require data from a single table, but others may use a variety of options to include data from multiple tables.

While Application Insights stores application data such as requests, exceptions, traces, and usage in Azure Monitor logs, this data is stored in a different partition than the other log data. You use the same query language to access this data but must use the Application Insights console or Application Insights REST API to access it. You can use cross-resources queries to combine Application Insights data with other log data in Azure Monitor.

![Orgnization-KQL](C:\Users\chboeh\Documents\GitHub\AzureSentinel\Training\Pictures\queries-tables.png)

