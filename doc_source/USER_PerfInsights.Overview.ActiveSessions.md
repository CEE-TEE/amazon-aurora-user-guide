# Database load<a name="USER_PerfInsights.Overview.ActiveSessions"></a>

Database load \(DB load\) measures the level of activity in your database\. The key metric in Performance Insights is `DBLoad`, which is collected every second\. The unit for the `DBLoad` metric is the average active sessions \(AAS\) for the DB engine\. 

**Topics**
+ [Average active sessions](#USER_PerfInsights.Overview.ActiveSessions.AAS)
+ [Average active executions](#USER_PerfInsights.Overview.ActiveSessions.AAE)
+ [Dimensions](#USER_PerfInsights.Overview.ActiveSessions.dimensions)

## Average active sessions<a name="USER_PerfInsights.Overview.ActiveSessions.AAS"></a>

An active session is a connection that has submitted work to the DB engine and is waiting for a response\. For example, if you submit a SQL query to the DB engine, the database session is active while the engine is processing the query\.

To obtain the average active sessions, Performance Insights samples the number of sessions concurrently running a query\. The AAS is the total number of sessions divided by the total number of samples\. The following table shows 5 consecutive samples of a running query\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_PerfInsights.Overview.ActiveSessions.html)

In the preceding example, the DB load for the time interval is 2 AAS\. An increase in DB load means that, on average, more sessions are running on the database\.

## Average active executions<a name="USER_PerfInsights.Overview.ActiveSessions.AAE"></a>

The average active executions \(AAE\) per second is related to AAS\. To calculate the AAE, Performance Insights divides the total execution time of a query by the time interval\. The following table shows the AAE calculation for the same query in the preceding table\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_PerfInsights.Overview.ActiveSessions.html)

In most cases, the AAS and AAE for a query are approximately the same\. However, because the inputs to the calculations are different data sources, the calculations often vary slightly\.

## Dimensions<a name="USER_PerfInsights.Overview.ActiveSessions.dimensions"></a>

The `db.load` metric is different from the other time\-series metrics because you can break it into subcomponents called dimensions\. You can think of dimensions as categories or "group by" clauses for the different characteristics of the `DB Load` metric\. When you are diagnosing performance issues, the most useful dimensions are wait events and top SQL\.

### Wait events<a name="USER_PerfInsights.Overview.ActiveSessions.waits"></a>

A *wait event* causes a SQL statement to wait for a specific event to happen before it can continue running\. For example, a SQL statement might wait until a locked resource is unlocked\. By combining `DB Load` with wait events, you can get a complete picture of the session state\. Wait events vary by DB engine: 
+ For a list of the most commonly used wait events for Aurora MySQL, see [Aurora MySQL wait events](AuroraMySQL.Reference.md#AuroraMySQL.Reference.Waitevents)\.
+ For information about all MySQL wait events, see [Wait Event Summary Tables](https://dev.mysql.com/doc/refman/5.7/en/wait-summary-tables.html) in the MySQL documentation\.
+ For a list of the most commonly used wait events for Aurora PostgreSQL, see [Amazon Aurora PostgreSQL wait events](AuroraPostgreSQL.Reference.Waitevents.md)\.
+ For information about all PostgreSQL wait events, see [PostgreSQL Wait Events](https://www.postgresql.org/docs/10/static/monitoring-stats.html#WAIT-EVENT-TABLE) in the PostgreSQL documentation\.

### Top SQL<a name="USER_PerfInsights.Overview.ActiveSessions.top-sql"></a>

Whereas wait events show bottlenecks, top SQL shows which queries are contributing the most to DB load\. For example, many queries might be currently running on the database, but a single query might consume 99% of the DB load\. In this case, the high load might indicate a problem with the query\. 

By default, the Performance Insights console displays top SQL queries that are contributing to the database load\. The console also shows relevant statistics for each statement\. To diagnose performance problems for a specific statement, you can examine its execution plan\.