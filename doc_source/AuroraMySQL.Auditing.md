# Using Advanced Auditing with an Amazon Aurora MySQL DB cluster<a name="AuroraMySQL.Auditing"></a>

You can use the high\-performance Advanced Auditing feature in Amazon Aurora MySQL to audit database activity\. To do so, you enable the collection of audit logs by setting several DB cluster parameters\. When Advanced Auditing is enabled, you can use it to log any combination of supported events\. You can view or download the audit logs to review them\.

**Note**  
You can publish Aurora MySQL general, slow, audit, and error log data to a log group in CloudWatch Logs\. For more information, see [Publishing Amazon Aurora MySQL logs to Amazon CloudWatch Logs](AuroraMySQL.Integrating.CloudWatch.md)\.

## Enabling Advanced Auditing<a name="AuroraMySQL.Auditing.Enable"></a>

Use the parameters described in this section to enable and configure Advanced Auditing for your DB cluster\. 

Use the `server_audit_logging` parameter to enable or disable Advanced Auditing, and the `server_audit_events` parameter to specify what events to log\. 

Use the `server_audit_excl_users` and `server_audit_incl_users` parameters to specify who gets audited\. If `server_audit_excl_users` and `server_audit_incl_users` are empty \(the default\), all users are audited\. If you add users to `server_audit_incl_users` and leave `server_audit_excl_users` empty, then only those users are audited\. If you add users to `server_audit_excl_users` and leave `server_audit_incl_users` empty, then only those users are not audited, and all other users are\. If you add the same users to both `server_audit_excl_users` and `server_audit_incl_users`, then those users are audited because `server_audit_incl_users` is given higher priority\.

Configure Advanced Auditing by setting these parameters in the parameter group used by your DB cluster\. You can use the procedure shown in [Modifying parameters in a DB parameter group](USER_WorkingWithParamGroups.md#USER_WorkingWithParamGroups.Modifying) to modify DB cluster parameters using the AWS Management Console\. You can use the [modify\-db\-cluster\-parameter\-group](https://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-cluster-parameter-group.html) AWS CLI command or the [ModifyDBClusterParameterGroup](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBClusterParameterGroup.html) Amazon RDS API command to modify DB cluster parameters programmatically\.

Modifying these parameters doesn't require a DB cluster restart\.

### server\_audit\_logging<a name="AuroraMySQL.Auditing.Enable.server_audit_logging"></a>

Enables or disables Advanced Auditing\. This parameter defaults to OFF; set it to ON to enable Advanced Auditing\.  

### server\_audit\_events<a name="AuroraMySQL.Auditing.Enable.server_audit_events"></a>

Contains the comma\-delimited list of events to log\. Events must be specified in all caps, and there should be no white space between the list elements, for example: `CONNECT,QUERY_DDL`\. This parameter defaults to an empty string\.

You can log any combination of the following events: 
+ CONNECT – Logs both successful and failed connections and also disconnections\. This event includes user information\.
+ QUERY – Logs all queries in plain text, including queries that fail due to syntax or permission errors\.
+ QUERY\_DCL – Similar to the QUERY event, but returns only data control language \(DCL\) queries \(GRANT, REVOKE, and so on\)\.
+ QUERY\_DDL – Similar to the QUERY event, but returns only data definition language \(DDL\) queries \(CREATE, ALTER, and so on\)\.
+ QUERY\_DML – Similar to the QUERY event, but returns only data manipulation language \(DML\) queries \(INSERT, UPDATE, and so on, and also SELECT\)\.
+ TABLE – Logs the tables that were affected by query execution\.

### server\_audit\_excl\_users<a name="AuroraMySQL.Auditing.Enable.server_audit_excl_users"></a>

Contains the comma\-delimited list of user names for users whose activity isn't logged\. There should be no white space between the list elements, for example: `rdsadmin,user_1,user_2`\. This parameter defaults to an empty string\. The maximum length is 1024 characters\. Specified user names must match corresponding values in the `User` column of the `mysql.user` table\. For more information about user names, see [the MySQL documentation](https://dev.mysql.com/doc/refman/5.6/en/user-names.html)\.

Connect and disconnect events aren't affected by this variable; they are always logged if specified\. A user is logged if that user is also specified in the `server_audit_incl_users` parameter, because that setting has higher priority than `server_audit_excl_users`\.

### server\_audit\_incl\_users<a name="AuroraMySQL.Auditing.Enable.server_audit_incl_users"></a>

Contains the comma\-delimited list of user names for users whose activity is logged\. There should be no white space between the list elements, for example: `user_3,user_4`\. This parameter defaults to an empty string\. The maximum length is 1024 characters\. Specified user names must match corresponding values in the `User` column of the `mysql.user` table\. For more information about user names, see [the MySQL documentation](https://dev.mysql.com/doc/refman/5.6/en/user-names.html)\.

Connect and disconnect events aren't affected by this variable; they are always logged if specified\. A user is logged even if that user is also specified in the `server_audit_excl_users` parameter, because `server_audit_incl_users` has higher priority\.  

## Viewing audit logs<a name="AuroraMySQL.Auditing.View"></a>

 You can view and download the audit logs by using the console\. On the **Databases** page, choose the DB instance to show its details, then scroll to the **Logs** section\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/aurora-log.png)

To download a log file, choose that file in the **Logs** section and then choose **Download**\.

You can also get a list of the log files by using the [describe\-db\-log\-files](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-log-files.html) AWS CLI command\. You can download the contents of a log file by using the [download\-db\-log\-file\-portion](https://docs.aws.amazon.com/cli/latest/reference/rds/download-db-log-file-portion.html) AWS CLI command\. For more information, see [Viewing and listing database log files](USER_LogAccess.Procedural.Viewing.md) and [Downloading a database log file](USER_LogAccess.Procedural.Downloading.md)\.

## Audit log details<a name="AuroraMySQL.Auditing.Logs"></a>

Log files are in UTF\-8 format\. The audit log is stored separately on the local \(ephemeral\) storage of each instance\. Each Aurora instance distributes writes across four log files at a time\. The maximum size of the logs is 100 MB in aggregate\. When this non\-configurable limit is reached, Aurora rotates the files and generates four new files\. 

Log file entries are not in sequential order\. To order the entries, use the timestamp value\. To see the latest events, you might have to review all log files\.

The audit log files include the following comma\-delimited information in rows, in the specified order:


| Field | Description | 
| --- | --- | 
|  timestamp  |  The Unix time stamp for the logged event with microsecond precision\.  | 
|  serverhost  |  The name of the instance that the event is logged for\.  | 
|  username  |  The connected user name of the user\.  | 
|  host  |  The host that the user connected from\.  | 
|  connectionid  |  The connection ID number for the logged operation\.  | 
|  queryid  |  The query ID number, which can be used for finding the relational table events and related queries\. For `TABLE` events, multiple lines are added\.  | 
|  operation  |  The recorded action type\. Possible values are: `CONNECT`, `QUERY`, `READ`, `WRITE`, `CREATE`, `ALTER`, `RENAME`, and `DROP`\.  | 
|  database  |  The active database, as set by the `USE` command\.  | 
|  object  |  For `QUERY` events, this value indicates the query that the database performed\. For `TABLE` events, it indicates the table name\.  | 
|  retcode  |  The return code of the logged operation\.  | 