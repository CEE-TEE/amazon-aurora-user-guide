# Setting the binary logging format<a name="USER_LogAccess.MySQL.BinaryFormat"></a>

MySQL on Amazon Aurora supports the *row\-based*, *statement\-based*, and *mixed* binary logging formats for MySQL version 5\.6 and later\. The default binary logging format is mixed\. For details on the different MySQL binary log formats, see [Binary logging formats](https://dev.mysql.com/doc/refman/8.0/en/binary-log-formats.html) in the MySQL documentation\.

If you plan to use replication, the binary logging format is important because it determines the record of data changes that is recorded in the source and sent to the replication targets\. For information about the advantages and disadvantages of different binary logging formats for replication, see [Advantages and disadvantages of statement\-based and row\-based replication](https://dev.mysql.com/doc/refman/8.0/en/replication-sbr-rbr.html) in the MySQL documentation\.

**Important**  
Setting the binary logging format to row\-based can result in very large binary log files\. Large binary log files reduce the amount of storage available for a DB cluster and can increase the amount of time to perform a restore operation of a DB cluster\.  
Statement\-based replication can cause inconsistencies between the source DB cluster and a read replica\. For more information, see [ Determination of safe and unsafe statements in binary logging](https://dev.mysql.com/doc/refman/8.0/en/replication-rbr-safe-unsafe.html) in the MySQL documentation\.

**To set the MySQL binary logging format**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. Choose the parameter group used by the DB cluster you want to modify\.

   You can't modify a default parameter group\. If the DB cluster is using a default parameter group, create a new parameter group and associate it with the DB cluster\.

   For more information on parameter groups, see [Working with DB parameter groups and DB cluster parameter groups](USER_WorkingWithParamGroups.md)\.

1. From **Parameter group actions**, choose **Edit**\.

1. Set the `binlog_format` parameter to the binary logging format of your choice \(**ROW**, **STATEMENT**, or **MIXED**\)\.

1. Choose **Save changes** to save the updates to the DB cluster parameter group\.

**Important**  
Changing a DB cluster parameter group affects all DB clusters that use that parameter group\. If you want to specify different binary logging formats for different Aurora MySQL DB clusters in an AWS Region, the DB clusters must use different DB cluster parameter groups\. These parameter groups identify different logging formats\. Assign the appropriate DB cluster parameter group to each DB clusters\. For more information about Aurora MySQL parameters, see [Aurora MySQL parameters](AuroraMySQL.Reference.md#AuroraMySQL.Reference.ParameterGroups)\.