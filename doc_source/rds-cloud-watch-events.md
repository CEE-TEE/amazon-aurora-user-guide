# Getting CloudWatch Events and Amazon EventBridge events for Amazon Aurora<a name="rds-cloud-watch-events"></a>

Amazon CloudWatch Events and Amazon EventBridge enable you to automate AWS services and respond to system events such as application availability issues or resource changes\. Events from AWS services are delivered to CloudWatch Events and EventBridge in near real time\. You can write simple rules to indicate which events interest you and what automated actions to take when an event matches a rule\.

**Note**  
Amazon RDS emits events on a best effort basis\. We recommend that you avoid writing programs that depends on the order or existence of notification events, as they might be out of sequence or missing\. 

You can set a variety of targets, such as an AWS Lambda function or an Amazon SNS topic, which receive events in JSON format\. For more information, see the [Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) and the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.

For example, you can configure Amazon Aurora to send events to CloudWatch Events or Amazon EventBridge whenever a DB instance is created or deleted\.

**Topics**
+ [Creating rules to send Amazon RDS events to CloudWatch Events](#rds-cloudwatch-events.sending-to-cloudwatch-events)
+ [DB cluster events](#rds-cloudwatch-events.db-clusters)
+ [DB instance events](#rds-cloudwatch-events.db-instances)
+ [DB parameter group events](#rds-cloudwatch-events.db-parameter-groups)
+ [DB security group events](#rds-cloudwatch-events.db-security-groups)
+ [DB cluster snapshot events](#rds-cloudwatch-events.db-cluster-snapshots)
+ [DB snapshot events](#rds-cloudwatch-events.db-snapshots)

## Creating rules to send Amazon RDS events to CloudWatch Events<a name="rds-cloudwatch-events.sending-to-cloudwatch-events"></a>

You can create CloudWatch Events rules to send Amazon RDS events to CloudWatch Events\.

Use the following steps to create a CloudWatch Events rule that triggers on an event emitted by an AWS service\.

**To create a rule that triggers on an event:**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Under **Events** in the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. For **Event Source**, do the following:

   1. Choose **Event Pattern**\.

   1. For **Service Name**, choose **Relational Database Service \(RDS\)**\.

   1. For **Event Type**, choose the type of Amazon RDS resource that triggers the event\. For example, if a DB instance triggers the event, choose **RDS DB Instance Event**\.

1. For **Targets**, choose **Add Target** and choose the AWS service that is to act when an event of the selected type is detected\. 

1. In the other fields in this section, enter information specific to this target type, if any is needed\. 

1. For many target types, CloudWatch Events needs permissions to send events to the target\. In these cases, CloudWatch Events can create the IAM role needed for your event to run: 
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created before, choose **Use existing role**\.

1. Optionally, repeat steps 5\-7 to add another target for this rule\.

1. Choose **Configure details**\. For **Rule definition**, type a name and description for the rule\.

   The rule name must be unique within this Region\.

1. Choose **Create rule**\.

For more information, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) in the *Amazon CloudWatch User Guide*\.

## DB cluster events<a name="rds-cloudwatch-events.db-clusters"></a>

The following is an example of a DB cluster event\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "RDS DB Cluster Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:cluster:my-db-cluster"
  ],
  "detail": {
    "EventCategories": [
      "notification"
    ],
    "SourceType": "CLUSTER",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:cluster:my-db-cluster",
    "Date": "2018-10-06T12:26:13.882Z",
    "SourceIdentifier": "rds:my-db-cluster",
    "Message": "Database cluster has been patched"
  }
}
```

## DB instance events<a name="rds-cloudwatch-events.db-instances"></a>

The following is an example of a DB instance event\.

```
{
  "version": "0",
  "id": "68f6e973-1a0c-d37b-f2f2-94a7f62ffd4e",
  "detail-type": "RDS DB Instance Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-09-27T22:36:43Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:db:my-db-instance"
  ],
  "detail": {
    "EventCategories": [
      "failover"
    ],
    "SourceType": "DB_INSTANCE",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:db:my-db-instance",
    "Date": "2018-09-27T22:36:43.292Z",
    "SourceIdentifier": "rds:my-db-instance",
    "Message": "A Multi-AZ failover has completed."
  }
}
```

## DB parameter group events<a name="rds-cloudwatch-events.db-parameter-groups"></a>

The following is an example of a DB parameter group event\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "RDS DB Parameter Group Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:pg:my-db-param-group"
  ],
  "detail": {
    "EventCategories": [
      "configuration change"
    ],
    "SourceType": "DB_PARAM",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:pg:my-db-param-group",
    "Date": "2018-10-06T12:26:13.882Z",
    "SourceIdentifier": "rds:my-db-param-group",
    "Message": "Updated parameter time_zone to UTC with apply method immediate"
  }
}
```

## DB security group events<a name="rds-cloudwatch-events.db-security-groups"></a>

The following is an example of a DB security group event\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "RDS DB Security Group Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:secgrp:my-security-group"
  ],
  "detail": {
    "EventCategories": [
      "configuration change"
    ],
    "SourceType": "SECURITY_GROUP",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:secgrp:my-security-group",
    "Date": "2018-10-06T12:26:13.882Z",
    "SourceIdentifier": "rds:my-security-group",
    "Message": "Applied change to security group"
  }
}
```

## DB cluster snapshot events<a name="rds-cloudwatch-events.db-cluster-snapshots"></a>

The following is an example of a DB cluster snapshot event\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "RDS DB Cluster Snapshot Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:cluster-snapshot:rds:my-db-cluster-snapshot"
  ],
  "detail": {
    "EventCategories": [
      "backup"
    ],
    "SourceType": "CLUSTER_SNAPSHOT",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:cluster-snapshot:rds:my-db-cluster-snapshot",
    "Date": "2018-10-06T12:26:13.882Z",
    "SourceIdentifier": "rds:my-db-cluster-snapshot",
    "Message": "Creating manual cluster snapshot"
  }
}
```

## DB snapshot events<a name="rds-cloudwatch-events.db-snapshots"></a>

The following is an example of a DB snapshot event\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "RDS DB Snapshot Event",
  "source": "aws.rds",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:rds:us-east-1:123456789012:snapshot:rds:my-db-snapshot"
  ],
  "detail": {
    "EventCategories": [
      "deletion"
    ],
    "SourceType": "SNAPSHOT",
    "SourceArn": "arn:aws:rds:us-east-1:123456789012:snapshot:rds:my-db-snapshot",
    "Date": "2018-10-06T12:26:13.882Z",
    "SourceIdentifier": "rds:my-db-snapshot",
    "Message": "Deleted manual snapshot"
  }
}
```