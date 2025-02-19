# Analyzing performance anomalies with Amazon DevOps Guru for Amazon RDS<a name="devops-guru-for-rds"></a>

Amazon DevOps Guru is a fully managed operations service that helps developers and operators improve the performance and availability of their applications\. DevOps Guru offloads the tasks associated with identifying operational issues so that you can quickly implement recommendations to improve your application\. To learn more, see [What is Amazon DevOps Guru?](https://docs.aws.amazon.com/devops-guru/latest/userguide/welcome.html) in the *Amazon DevOps Guru User Guide*\.

DevOps Guru detects, analyzes, and makes recommendations for operational issues for all Amazon RDS DB engines\. DevOps Guru for RDS extends this capability by applying machine learning to Performance Insights metrics for Amazon Aurora databases\. These monitoring features allow DevOps Guru for RDS to detect and diagnose performance bottlenecks and recommend specific corrective actions\. To learn more, see [Overview of DevOps Guru for RDS](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-rds.overview.html) in the *Amazon DevOps Guru User Guide*\.

The following video is an overview of DevOps Guru for RDS\.

[![AWS Videos](http://img.youtube.com/vi/N3NNYgzYUDA/0.jpg)](http://www.youtube.com/watch?v=N3NNYgzYUDA)

For a deep dive on this subject, see [Amazon DevOps Guru for RDS under the hood\.](http://aws.amazon.com/blogs/database/amazon-devops-guru-for-rds-under-the-hood/)

**Topics**
+ [Benefits of DevOps Guru for RDS](#devops-guru-for-rds.benefits)
+ [How DevOps Guru for RDS works](#devops-guru-for-rds.how-it-works)
+ [Setting up DevOps Guru for RDS](#devops-guru-for-rds.configuring)

## Benefits of DevOps Guru for RDS<a name="devops-guru-for-rds.benefits"></a>

If you're responsible for an Amazon Aurora database, you might not know that an event or regression that is affecting that database is occurring\. When you learn about the issue, you might not know why it's occurring or what to do about it\. Rather than turning to a database administrator \(DBA\) for help or relying on third\-party tools, you can follow recommendations from DevOps Guru for RDS\. 

You gain the following advantages from the detailed analysis of DevOps Guru for RDS:

**Fast diagnosis**  
DevOps Guru for RDS continuously monitors and analyzes database telemetry\. Performance Insights, Enhanced Monitoring, and Amazon CloudWatch collect telemetry data for your database cluster\. DevOps Guru for RDS uses statistical and machine learning techniques to mine this data and detect anomalies\. To learn more about telemetry data, see [Monitoring DB load with Performance Insights on Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_PerfInsights.html) and [Monitoring the OS by using Enhanced Monitoring](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_Monitoring.OS.html) in the *Amazon Aurora User Guide*\.

**Fast resolution**  
Each anomaly identifies the performance issue and suggests avenues of investigation or corrective actions\. For example, DevOps Guru for RDS might recommend that you investigate specific wait events\. Or it might recommend that you tune your application pool settings to limit the number of database connections\. Based on these recommendations, you can resolve performance issues more quickly than by troubleshooting manually\.

**Deep knowledge of Amazon engineers**  
To detect performance issues and help you resolve bottlenecks, DevOps Guru for RDS relies on machine learning \(ML\)\. Amazon database engineers contributed to the development of the DevOps Guru for RDS findings, which encapsulate many years of managing hundreds of thousands of databases\. By drawing on this collective knowledge, DevOps Guru for RDS can teach you best practices\.

## How DevOps Guru for RDS works<a name="devops-guru-for-rds.how-it-works"></a>

DevOps Guru for RDS collects data about your Aurora databases from Amazon RDS Performance Insights\. The most important metric is `DBLoad`\. DevOps Guru for RDS consumes the Performance Insights metrics, analyzes them with machine learning, and publishes insights to the dashboard\.

### Insights<a name="devops-guru-for-rds.how-it-works.insights"></a>

An *insight* is a collection of related anomalies that were detected by DevOps Guru\. If DevOps Guru for RDS finds performance issues in your Amazon Aurora DB instances, it publishes an insight in the DevOps Guru dashboard\. To learn more about insights, see [Working with insights in DevOps Guru](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-insights.html) in the *Amazon DevOps Guru User Guide*\.

### Anomalies<a name="devops-guru-for-rds.how-it-works.anomalies"></a>

In DevOps Guru for RDS, an *anomaly* is a pattern that deviates from what is considered normal performance for your Amazon Aurora database\. 

#### Causal anomalies<a name="devops-guru-for-rds.how-it-works.anomalies.causal"></a>

A *causal anomaly* is a top\-level anomaly within an insight\. **Database load \(DB load\)** is the causal anomaly for DevOps Guru for RDS\. 

An anomaly measures performance impact by assigning a severity level of **High**, **Medium**, or **Low**\. To learn more, see [Key concepts for DevOps Guru for RDS](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-rds.overview.definitions.html) in the *Amazon DevOps Guru User Guide*\.

If DevOps Guru detects an anomaly on your DB instance, you're alerted in the **Databases** page of the RDS console\. To go to the anomaly page from the RDS console, choose the link in the alert message\. The RDS console also alerts you in the page for your Amazon Aurora cluster\.

#### Contextual anomalies<a name="devops-guru-for-rds.how-it-works.anomalies.contextual"></a>

A *contextual anomaly* is a finding within **Database load \(DB load\)**\. Each contextual anomaly describes a specific Amazon Aurora performance issue that requires investigation\. For example, DevOps Guru for RDS might recommend that you consider increasing CPU capacity or investigate wait events that are contributing to DB load\.

**Important**  
We recommend that you test any changes on a test instance before modifying a production instance\. In this way, you understand the impact of the change\.

To learn more, see [Analyzing anomalies in Amazon Aurora clusters](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-rds.analyzing.html) in the *Amazon DevOps Guru User Guide*\.

## Setting up DevOps Guru for RDS<a name="devops-guru-for-rds.configuring"></a>

To allow DevOps Guru for Amazon RDS to publish insights for an Amazon Aurora database, complete the following tasks\.

**Topics**
+ [Configuring IAM access policies for DevOps Guru for RDS](#devops-guru-for-rds.configuring.access)
+ [Turning on Performance Insights for your Aurora DB instances](#devops-guru-for-rds.configuring.performance-insights)
+ [Turning on DevOps Guru and specifying resource coverage](#devops-guru-for-rds.configuring.coverage)

### Configuring IAM access policies for DevOps Guru for RDS<a name="devops-guru-for-rds.configuring.access"></a>

To view alerts from DevOps Guru in the RDS console, your AWS Identity and Access Management \(IAM\) user or role must have either of the following policies:
+ The AWS managed policy `AmazonDevOpsGuruConsoleFullAccess`
+ The AWS managed policy `AmazonDevOpsGuruConsoleReadOnlyAccess` and either of the following policies:
  + The AWS managed policy `AmazonRDSFullAccess`
  + A customer managed policy that includes `pi:GetResourceMetrics` and `pi:DescribeDimensionKeys`

For more information, see [Configuring access policies for Performance Insights](USER_PerfInsights.access-control.md)\.

### Turning on Performance Insights for your Aurora DB instances<a name="devops-guru-for-rds.configuring.performance-insights"></a>

DevOps Guru for RDS relies on Performance Insights for its data\. Without Performance Insights, DevOps Guru publishes anomalies, but doesn't include the detailed analysis and recommendations\.

When you create an Aurora DB cluster or modify a cluster instance, you can turn on Performance Insights\. For more information, see [Turning Performance Insights on and off](USER_PerfInsights.Enabling.md)\.

### Turning on DevOps Guru and specifying resource coverage<a name="devops-guru-for-rds.configuring.coverage"></a>

You can turn on DevOps Guru to have it monitor your Aurora databases in either of the following ways\.

**Topics**
+ [Turning on DevOps Guru in the RDS console](#devops-guru-for-rds.configuring.coverage.rds-console)
+ [Adding Aurora resources on the DevOps Guru console](#devops-guru-for-rds.configuring.coverage.guru-console)

#### Turning on DevOps Guru in the RDS console<a name="devops-guru-for-rds.configuring.coverage.rds-console"></a>

You can take multiple paths in the Amazon RDS console to turn on DevOps Guru\.

**Topics**
+ [Turning on DevOps Guru when you create an Aurora database](#devops-guru-for-rds.configuring.coverage.rds-console.create)
+ [Turning on DevOps Guru from the notification banner](#devops-guru-for-rds.configuring.coverage.rds-console.existing)
+ [Responding to a permissions error when you turn on DevOps Guru](#devops-guru-for-rds.configuring.coverage.rds-console.error)

##### Turning on DevOps Guru when you create an Aurora database<a name="devops-guru-for-rds.configuring.coverage.rds-console.create"></a>

The creation workflow includes a setting that turns on DevOps Guru coverage for your database\. This setting is turned on by default when you choose the **Production** template\.

**To turn on DevOps Guru when you create an Aurora database**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Follow the steps in [Creating a DB cluster](Aurora.CreateInstance.md#Aurora.CreateInstance.Creating), up to but not including the step where you choose monitoring settings\.

1. In **Monitoring**, choose **Turn on Performance Insights**\. For DevOps Guru for RDS to provide detailed analysis of performance anomalies, Performance Insights must be turned on\.

1. Choose **Turn on DevOps Guru**\.  
![\[Turn on DevOps Guru when you create a DB cluster\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-enable-create.png)

1. Create a tag for your database so that DevOps Guru can monitor it\. Do the following:
   + In the text field for **Tag key**, enter a name that begins with **Devops\-Guru\-**\.
   + In the text field for **Tag value**, enter any value\. For example, if you enter **rds\-database\-1** for the name of your Aurora database, you can also enter **rds\-database\-1** as the tag value\.

   For more information about tags, see "[Use tags to identify resources in your DevOps Guru applications](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-resource-tags.html)" in the *Amazon DevOps Guru User Guide*\.

1. Complete the remaining steps in [Creating a DB cluster](Aurora.CreateInstance.md#Aurora.CreateInstance.Creating)\.

##### Turning on DevOps Guru from the notification banner<a name="devops-guru-for-rds.configuring.coverage.rds-console.existing"></a>

If your resources aren't covered by DevOps Guru, Amazon RDS notifies you with a banner in the following locations:
+ The **Monitoring** tab of a DB cluster instance
+ The Performance Insights dashboard

![\[DevOps Guru banner\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-enable-banner.png)

**To turn on DevOps Guru for your Aurora database**

1. In the banner, choose **Turn on DevOps Guru for RDS**\. 

1. Enter a tag key name and value\. For more information about tags, see "[Use tags to identify resources in your DevOps Guru applications](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-resource-tags.html)" in the *Amazon DevOps Guru User Guide*\.  
![\[Turn on DevOps Guru in the RDS console\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/perf_insights_devops_guru.png)

1. Choose **Turn on DevOps Guru**\.

In the Performance Insights dashboard, you can also turn off DevOps Guru\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-pi-toggle.png)

##### Responding to a permissions error when you turn on DevOps Guru<a name="devops-guru-for-rds.configuring.coverage.rds-console.error"></a>

If you turn on DevOps Guru from the RDS console when you create a database, RDS might display the following banner about missing permissions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-permissions-error.png)

**To respond to a permissions error**

1. Grant your IAM user or role the user managed role `AmazonDevOpsGuruConsoleFullAccess`\. For more information, see [Configuring IAM access policies for DevOps Guru for RDS](#devops-guru-for-rds.configuring.access)\.

1. Open the RDS console\.

1. In the navigation pane, choose **Performance Insights**\.

1. Choose a DB instance in the cluster that you just created\.

1. Turn on **DevOps Guru for RDS**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-pi-toggle-off.png)

1. Choose a tag value\. For more information, see "[Use tags to identify resources in your DevOps Guru applications](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-resource-tags.html)" in the *Amazon DevOps Guru User Guide*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/devops-guru-turn-on.png)

1. Choose **Turn on DevOps Guru**\.

#### Adding Aurora resources on the DevOps Guru console<a name="devops-guru-for-rds.configuring.coverage.guru-console"></a>

You can specify your DevOps Guru resource coverage on the DevOps Guru console\. Follow the step described in [Specify your DevOps Guru resource coverage](https://docs.aws.amazon.com/devops-guru/latest/userguide/choose-coverage.html) in the *Amazon DevOps Guru User Guide*\. When you edit your analyzed resources, choose one of the following options:
+ Choose **All account resources** to analyze all supported resources, including the Aurora databases, in your AWS account and Region\.
+ Choose **CloudFormation stacks** to analyze the Aurora databases that are in stacks you choose\. For more information, see [Use AWS CloudFormation stacks to identify resources in your DevOps Guru applications](https://docs.aws.amazon.com/https:/devops-guru/latest/userguide/working-with-cfn-stacks.html) in the *Amazon DevOps Guru User Guide*\.
+ Choose **Tags** to analyze the Aurora databases that you have tagged\. For more information, see [Use tags to identify resources in your DevOps Guru applications](https://docs.aws.amazon.com/devops-guru/latest/userguide/working-with-resource-tags.html) in the *Amazon DevOps Guru User Guide*\.

For more information, see [Enable DevOps Guru](https://docs.aws.amazon.com/devops-guru/latest/userguide/getting-started-enable-service.html) in the *Amazon DevOps Guru User Guide*\.