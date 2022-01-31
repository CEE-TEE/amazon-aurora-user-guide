# Overview of the Performance Insights dashboard<a name="USER_PerfInsights.UsingDashboard.Components"></a>

The dashboard is the easiest way to interact with Performance Insights\. The following example shows the dashboard for a MySQL DB instance\. By default, the Performance Insights dashboard shows data for the last hour\.

![\[Enable Performance Insights during DB instance creation with console\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/aurora_perf_insights_enabling.png)

The dashboard is divided into the following parts:

1. **Counter Metrics** – Shows data for specific performance counter metrics\.

1. **DB Load Chart** – Shows how the DB load compares to DB instance capacity as represented by the **Max vCPU** line\.

1.  **Top *items*** – Shows the top dimensions contributing to DB load\.

**Topics**
+ [Counter metrics chart](#USER_PerfInsights.UsingDashboard.Components.Countermetrics)
+ [Database load chart](#USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions)
+ [Top dimensions table](#USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions.TopLoadItemsTable)

## Counter metrics chart<a name="USER_PerfInsights.UsingDashboard.Components.Countermetrics"></a>

 The **Counter metrics** chart displays data for performance counters\. The default metrics depend on the DB engine:
+ Aurora MySQL– `db.SQL.Innodb_rows_read.avg`
+ Aurora PostgreSQL – `db.Transactions.xact_commit.avg`

![\[Counter metrics\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/oracle_perf_insights_counters.png)

To change the performance counters, choose **Manage Metrics**\. You can select multiple **OS metrics** or **Database metrics**, as shown in the following screenshot\. To see details for any metric, hover over the metric name\.

![\[Filter metrics\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_select_metrics.png)

For more information, see [Adding counter metrics to the Performance Insights dashboard](USER_PerfInsights_Counters.md)\.

## Database load chart<a name="USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions"></a>

The **Database load** chart shows how the database activity compares to DB instance capacity as represented by the **Max vCPU** line\. By default, the stacked line chart represents DB load as average active sessions per unit of time\. The DB load is sliced \(grouped\) by wait states\. 

![\[Database load\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_2.png)

### DB load sliced by dimensions<a name="USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions.dims"></a>

You can choose to display load as active sessions grouped by any supported dimensions\. The following table shows which dimensions are supported for the different engines\.


| Dimension | Aurora PostgreSQL | Aurora MySQL | 
| --- | --- | --- | 
|  Host  |  Yes  |  Yes  | 
|  SQL  |  Yes  |  Yes  | 
|  User  |  Yes  |  Yes  | 
|  Waits  |  Yes  |  Yes  | 
|  Application  |  Yes  |  No  | 
|  Database  |  Yes  |  Yes  | 
|  Session type  |  Yes  |  No  | 

The following image shows the dimensions for a PostgreSQL DB instance\.

![\[Filter metrics\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_2b.png)

### DB load details for a dimension item<a name="USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions.item-details"></a>

To see details about a DB load item within a dimension, hover over the item name\. The following image shows details for a SQL statement\.

![\[Database load item details\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_2c.png)

To see details for any item for the selected time period in the legend, hover over that item\.

![\[Time period details for DB load\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_3.png)

## Top dimensions table<a name="USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions.TopLoadItemsTable"></a>

The Top dimensions table slices DB load by different dimensions\. A dimension is a category or "slice by" for different characteristics of DB load\. If the dimension is SQL, **Top SQL** shows the SQL statements that contribute the most to DB load\.

![\[Top N dimensions\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/./images/perf_insights_4c.png)

Choose any of the following dimension tabs\.


| Tab | Description | Supported engines | 
| --- | --- | --- | 
|  Top SQL  |  The SQL statements that are currently running  |  All  | 
|  Top waits  |  The event for which the database backend is waiting  |  All  | 
|  Top hosts  |  The host name of the connected client  |  All  | 
|  Top users  |  The user logged in to the database  |  All  | 
|  The name of the database to which the client is connected  |    | 
|  Top applications  |  The name of the application that is connected to the database  |  Aurora PostgreSQL only  | 
|  Top session types  |  The type of the current session  | Aurora PostgreSQL only | 

To learn how to analyze queries by using the **Top SQL** tab, see [Overview of the Top SQL tab](USER_PerfInsights.UsingDashboard.Components.AvgActiveSessions.TopLoadItemsTable.TopSQL.md)\.