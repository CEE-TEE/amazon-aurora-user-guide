# Viewing Aurora Serverless v1 DB clusters<a name="aurora-serverless.viewing"></a>

 After you create one or more Aurora Serverless v1 DB clusters, you can view which DB clusters are type **Serverless** and which are type **Instance**\. You can also view the current number of Aurora capacity units \(ACUs\) each Aurora Serverless v1 DB cluster is using\. Each ACU is a combination of processing \(CPU\) and memory \(RAM\) capacity\. 

**To view your Aurora Serverless v1 DB clusters**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1.  In the upper\-right corner of the AWS Management Console, choose the AWS Region in which you created the Aurora Serverless v1 DB clusters\. 

1.  In the navigation pane, choose **Databases**\. 

    For each DB cluster, the DB cluster type is shown under **Role**\. The Aurora Serverless v1 DB clusters show **Serverless** for the type\. You can view an Aurora Serverless v1 DB cluster's current capacity under **Size**\.   
![\[Viewing Aurora Serverless v1 DB clusters\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/aurora-serverless-viewing.png)

1.  Choose the name of an Aurora Serverless v1 DB cluster to display its details\. 

    On the **Connectivity & security** tab, note the database endpoint\. Use this endpoint to connect to your Aurora Serverless v1 DB cluster\.   
![\[Viewing Aurora Serverless v1 DB cluster database endpoint\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/aurora-serverless-endpoint.png)

    Choose the **Configuration** tab to view the capacity settings\.   
![\[Viewing Aurora Serverless v1 DB cluster capacity settings\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/aurora-serverless-capacity-settings.png)

    A *scaling event *is generated when the DB cluster scales up, scales down, pauses, or resumes\. Choose the **Logs & events** tab to see recent events\. The following image shows examples of these events\.   
![\[Viewing Aurora Serverless v1 DB cluster capacity settings\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/aurora-serverless-scaling.png)

## Monitoring capacity and scaling events for your Aurora Serverless v1 DB cluster<a name="aurora-serverless.viewing.monitoring"></a>

 You can view your Aurora Serverless v1 DB cluster in CloudWatch to monitor the capacity allocated to the DB cluster with the `ServerlessDatabaseCapacity` metric\. You can also monitor all of the standard Aurora CloudWatch metrics, such as `CPUUtilization`, `DatabaseConnections`, `Queries`, and so on\. 

 You can have Aurora publish some or all database logs to CloudWatch\. You select the logs to publish by enabling the [configuration parameters such as `general_log` and `slow_query_log` in the DB cluster parameter group](aurora-serverless-v1.how-it-works.md#aurora-serverless.parameter-groups) associated with theAurora Serverless v1 cluster\. Unlike provisioned clusters, Aurora Serverless v1 clusters don't require you to specify in the DB cluster settings which log types to upload to CloudWatch\. Aurora Serverless v1 clusters automatically upload all the available logs\. When you disable a log configuration parameter, publishing of the log to CloudWatch stops\. You can also delete the logs in CloudWatch if they are no longer needed\. 

 To get started with Amazon CloudWatch for your Aurora Serverless v1 DB cluster, see [Viewing Aurora Serverless v1 logs with Amazon CloudWatch](aurora-serverless-v1.how-it-works.md#aurora-serverless.logging.monitoring)\. To learn more about how to monitor Aurora DB clusters through CloudWatch, see [Monitoring log events in Amazon CloudWatch](AuroraMySQL.Integrating.CloudWatch.md#AuroraMySQL.Integrating.CloudWatch.Monitor)\. 

 To connect to an Aurora Serverless v1 DB cluster, use the database endpoint\. For more information, see [Connecting to an Amazon Aurora DB cluster](Aurora.Connecting.md)\. 

**Note**  
 You can't connect directly to specific DB instances in your Aurora Serverless v1 DB clusters\. 