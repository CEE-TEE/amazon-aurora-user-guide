# Getting CloudWatch Events and Amazon EventBridge events for Amazon Aurora<a name="rds-cloud-watch-events"></a>

Using Amazon CloudWatch Events and Amazon EventBridge, you can automate AWS services and respond to system events such as application availability issues or resource changes\. 

**Topics**
+ [Overview of events for Aurora](#rds-cloudwatch-events.sample)
+ [Tutorial: log the state of an instance using EventBridge](#log-rds-instance-state)

## Overview of events for Aurora<a name="rds-cloudwatch-events.sample"></a>

An *event* indicates a change in an environment\. This can be an AWS environment, an SaaS partner service or application, or one of your own custom applications or services\. For example, Amazon Aurora generates an event when the state of an instance changes from pending to running\. Amazon Aurora deliver events to CloudWatch Events and EventBridge in near real time\. 

**Note**  
Amazon RDS emits events on a best effort basis\. We recommend that you avoid writing programs that depends on the order or existence of notification events, as they might be out of sequence or missing\. 

You can write simple rules to indicate which events interest you and what automated actions to take when an event matches a rule\. You can set a variety of targets, such as an AWS Lambda function or an Amazon SNS topic, which receive events in JSON format\. For example, you can configure Amazon Aurora to send events to CloudWatch Events or Amazon EventBridge whenever a DB instance is created or deleted\. For more information, see the [Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/) and the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.

**Topics**
+ [DB cluster events](#rds-cloudwatch-events.db-clusters)
+ [DB instance events](#rds-cloudwatch-events.db-instances)
+ [DB parameter group events](#rds-cloudwatch-events.db-parameter-groups)
+ [DB security group events](#rds-cloudwatch-events.db-security-groups)
+ [DB cluster snapshot events](#rds-cloudwatch-events.db-cluster-snapshots)
+ [DB snapshot events](#rds-cloudwatch-events.db-snapshots)

### DB cluster events<a name="rds-cloudwatch-events.db-clusters"></a>

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
    "Message": "Database cluster has been patched",
    "SourceIdentifier": "rds:my-db-cluster",
    "EventID": "RDS-EVENT-0173"
  }
}
```

### DB instance events<a name="rds-cloudwatch-events.db-instances"></a>

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
    "Message": "A Multi-AZ failover has completed.",
    "SourceIdentifier": "rds:my-db-instance",
    "EventID": "RDS-EVENT-0049"
  }
}
```

### DB parameter group events<a name="rds-cloudwatch-events.db-parameter-groups"></a>

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
    "Message": "Updated parameter time_zone to UTC with apply method immediate",
    "SourceIdentifier": "rds:my-db-param-group",
    "EventID": "RDS-EVENT-0037"
  }
}
```

### DB security group events<a name="rds-cloudwatch-events.db-security-groups"></a>

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
    "Message": "Applied change to security group",
    "SourceIdentifier": "rds:my-security-group",
    "EventID": "RDS-EVENT-0038"
  }
}
```

### DB cluster snapshot events<a name="rds-cloudwatch-events.db-cluster-snapshots"></a>

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
    "Message": "Creating manual cluster snapshot",
    "EventID": "RDS-EVENT-0074"
  }
}
```

### DB snapshot events<a name="rds-cloudwatch-events.db-snapshots"></a>

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
    "Message": "Deleted manual snapshot",
    "SourceIdentifier": "rds:my-db-snapshot",
    "EventID": "RDS-EVENT-0041"
  }
}
```

## Tutorial: log the state of an instance using EventBridge<a name="log-rds-instance-state"></a>

You can create an AWS Lambda function that logs the changes in state for an instance\. You can choose to create a rule that runs the function whenever there is a state transition or a transition to one or more states that are of interest\.

In this tutorial, you log any state change of an existing RDS DB instance\. The tutorial assumes that you have a small running test instance that you can shut down temporarily\.

**Important**  
Don't perform this tutorial on a running production instance\.

### Step 1: Create an AWS Lambda Function<a name="rds-create-lambda-function"></a>

Create a Lambda function to log the state change events\. You specify this function when you create your rule\.

**To create a Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. If you're new to Lambda, you see a welcome page\. Choose **Get Started Now**\. Otherwise, choose **Create function**\.

1. Choose **Author from scratch**\.

1. On the **Create function** page, do the following:

   1. Enter a name and description for the Lambda function\. For example, name the function **RDSInstanceStateChange**\. 

   1. In **Runtime**, select **Node\.js 14x**\. 

   1. In **Execution role**, choose **Create a new role with basic Lambda permissions**\. For **Existing role**, select your basic execution role\. Otherwise, create a basic execution role\.

   1. Choose **Create function**\.

1. On the **RDSInstanceStateChange** page, do the following:

   1. In **Code source**, select **index\.js**\. 

   1. Right\-click **index\.js**, and choose **Open**\.

   1. In the **index\.js** pane, delete the existing code\.

   1. Enter the following code:

      ```
      console.log('Loading function');
      
      exports.handler = async (event, context) => {
          console.log('Received event:', JSON.stringify(event));
      };
      ```

   1. Choose **Deploy**\.

### Step 2: Create a Rule<a name="rds-create-rule"></a>

Create a rule to run your Lambda function whenever you launch an Amazon RDS instance\.

**To create the EventBridge rule**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\. For example, enter **RDSInstanceStateChangeRule**\.

1. For **Define pattern**, do the following:

   1. Choose **Event pattern**\.

   1. Choose **Pre\-defined pattern by service**\.

   1. For **Service provider**, choose **AWS**\.

   1. For **Service Name**, choose **Relational Database Service \(RDS\)**\.

   1. For **Event type**, choose **RDS DB Instance Event**\.

1. For **Select event bus**, choose **AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Target**, choose **Lambda function**\.

1. For **Function**, select the Lambda function that you created\.

1. Choose **Create**\.

### Step 3: Test the Rule<a name="rds-test-rule"></a>

To test your rule, shut down an RDS DB instance\. After waiting a few minutes for the instance to shut down, verify that your Lambda function was invoked\.

**To test your rule by stopping a DB instance**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Stop an RDS DB instance\.

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, choose the name of the rule that you created\.

1. In **Rule details**, choose **Metrics for the rule**\.

   You are redirected to the Amazon CloudWatch console\.

1. In **All metrics**, choose the name of the rule that you created\.

   The graph should indicate that the rule was invoked\.

1. In the navigation pane, choose **Log groups**\.

1. Choose the name of the log group for your Lambda function \(**/aws/lambda/*function\-name***\)\.

1. Choose the name of the log stream to view the data provided by the function for the instance that you launched\. You should see a received event similar to the following:

   ```
   {
       "version": "0",
       "id": "12a345b6-78c9-01d2-34e5-123f4ghi5j6k",
       "detail-type": "RDS DB Instance Event",
       "source": "aws.rds",
       "account": "111111111111",
       "time": "2021-03-19T19:34:09Z",
       "region": "us-east-1",
       "resources": [
           "arn:aws:rds:us-east-1:111111111111:db:testdb"
       ],
       "detail": {
           "EventCategories": [
               "notification"
           ],
           "SourceType": "DB_INSTANCE",
           "SourceArn": "arn:aws:rds:us-east-1:111111111111:db:testdb",
           "Date": "2021-03-19T19:34:09.293Z",
           "Message": "DB instance stopped",
           "SourceIdentifier": "testdb",
           "EventID": "RDS-EVENT-0087"
       }
   }
   ```

1. \(Optional\) When you're finished, you can open the Amazon RDS console and start the instance that you stopped\.