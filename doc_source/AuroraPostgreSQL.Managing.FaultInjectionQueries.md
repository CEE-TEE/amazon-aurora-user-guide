# Testing Amazon Aurora PostgreSQL by using fault injection queries<a name="AuroraPostgreSQL.Managing.FaultInjectionQueries"></a>

You can test the fault tolerance of your Aurora PostgreSQL DB cluster by using fault injection queries\. Fault injection queries are issued as SQL commands to an Amazon Aurora instance\. Fault injection queries let you crash the instance so that you can test failover and recovery\. You can also simulate Aurora Replica failure, disk failure, and disk congestion\. Fault injection queries are supported by all available Aurora PostgreSQL versions, as follows\. 
+ Aurora PostgreSQL versions 12, 13, 14, and higher
+ Aurora PostgreSQL version 11\.7 and higher
+ Aurora PostgreSQL version 10\.11 and higher

**Topics**
+ [Testing an instance crash](#AuroraPostgreSQL.Managing.FaultInjectionQueries.Crash)
+ [Testing an Aurora Replica failure](#AuroraPostgreSQL.Managing.FaultInjectionQueries.ReplicaFailure)
+ [Testing a disk failure](#AuroraPostgreSQL.Managing.FaultInjectionQueries.DiskFailure)
+ [Testing disk congestion](#AuroraPostgreSQL.Managing.FaultInjectionQueries.DiskCongestion)

When a fault injection query specifies a crash, it forces a crash of the Aurora PostgreSQL DB instance\. The other fault injection queries result in simulations of failure events, but don't cause the event to occur\. When you submit a fault injection query, you also specify an amount of time for the failure event simulation to occur\.

You can submit a fault injection query to one of your Aurora Replica instances by connecting to the endpoint for the Aurora Replica\. For more information, see [Amazon Aurora connection management](Aurora.Overview.Endpoints.md)\.

## Testing an instance crash<a name="AuroraPostgreSQL.Managing.FaultInjectionQueries.Crash"></a>

You can force a crash of an Aurora PostgreSQL instance by using the fault injection query function `aurora_inject_crash()`\.

For this fault injection query, a failover does not occur\. If you want to test a failover, then you can choose the **Failover** instance action for your DB cluster in the RDS console, or use the [failover\-db\-cluster](https://docs.aws.amazon.com/cli/latest/reference/rds/failover-db-cluster.html) AWS CLI command or the [FailoverDBCluster](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_FailoverDBCluster.html) RDS API operation\. 

**Syntax**

```
1. SELECT aurora_inject_crash ('instance' | 'dispatcher' | 'node');
```Options

This fault injection query takes one of the following crash types\. The crash type is not case sensitive:

*'instance'*  
A crash of the PostgreSQL\-compatible database for the Amazon Aurora instance is simulated\.

*'dispatcher'*  
A crash of the dispatcher on the primary instance for the Aurora DB cluster is simulated\. The *dispatcher* writes updates to the cluster volume for an Amazon Aurora DB cluster\.

*'node'*  
A crash of both the PostgreSQL\-compatible database and the dispatcher for the Amazon Aurora instance is simulated\.

## Testing an Aurora Replica failure<a name="AuroraPostgreSQL.Managing.FaultInjectionQueries.ReplicaFailure"></a>

You can simulate the failure of an Aurora Replica by using the fault injection query function `aurora_inject_replica_failure()`\.

An Aurora Replica failure blocks replication to the Aurora Replica or all Aurora Replicas in the DB cluster by the specified percentage for the specified time interval\. When the time interval completes, the affected Aurora Replicas are automatically synchronized with the primary instance\.

**Syntax**

```
1. SELECT aurora_inject_replica_failure(
2.    percentage_of_failure, 
3.    time_interval, 
4.    'replica_name'
5. );
```Options

This fault injection query takes the following parameters:

*percentage\_of\_failure*  
The percentage of replication to block during the failure event\. This value can be a double between 0 and 100\. If you specify 0, then no replication is blocked\. If you specify 100, then all replication is blocked\.

*time\_interval*  
The amount of time to simulate the Aurora Replica failure\. The interval is in seconds\. For example, if the value is 20, the simulation runs for 20 seconds\.  
Take care when specifying the time interval for your Aurora Replica failure event\. If you specify too long an interval, and your writer instance writes a large amount of data during the failure event, then your Aurora DB cluster might assume that your Aurora Replica has crashed and replace it\.

*replica\_name*  
The Aurora Replica in which to inject the failure simulation\. Specify the name of an Aurora Replica to simulate a failure of the single Aurora Replica\. Specify an empty string to simulate failures for all Aurora Replicas in the DB cluster\.   
To identify replica names, see the `server_id` column from the `aurora_replica_status()` function\. For example:  

```
postgres=> SELECT server_id FROM aurora_replica_status();
```

## Testing a disk failure<a name="AuroraPostgreSQL.Managing.FaultInjectionQueries.DiskFailure"></a>

You can simulate a disk failure for an Aurora PostgreSQL DB cluster by using the fault injection query function `aurora_inject_disk_failure()`\.

During a disk failure simulation, the Aurora PostgreSQL DB cluster randomly marks disk segments as faulting\. Requests to those segments are blocked for the duration of the simulation\.

**Syntax**

```
1. SELECT aurora_inject_disk_failure(
2.    percentage_of_failure, 
3.    index, 
4.    is_disk, 
5.    time_interval
6. );
```Options

This fault injection query takes the following parameters:

*percentage\_of\_failure*  
The percentage of the disk to mark as faulting during the failure event\. This value can be a double between 0 and 100\. If you specify 0, then none of the disk is marked as faulting\. If you specify 100, then the entire disk is marked as faulting\.

*index*  
A specific logical block of data in which to simulate the failure event\. If you exceed the range of available logical blocks or storage nodes data, you receive an error that tells you the maximum index value that you can specify\. To avoid this error, see [Displaying volume status for an Aurora PostgreSQL DB cluster](AuroraPostgreSQL.Managing.VolumeStatus.md)\.

*is\_disk*  
Indicates whether the injection failure is to a logical block or a storage node\. Specifying true means injection failures are to a logical block\. Specifying false means injection failures are to a storage node\.

*time\_interval*  
The amount of time to simulate the Aurora Replica failure\. The interval is in seconds\. For example, if the value is 20, the simulation runs for 20 seconds\.

## Testing disk congestion<a name="AuroraPostgreSQL.Managing.FaultInjectionQueries.DiskCongestion"></a>

You can simulate a disk failure for an Aurora PostgreSQL DB cluster by using the fault injection query function `aurora_inject_disk_congestion()`\.

During a disk congestion simulation, the Aurora PostgreSQL DB cluster randomly marks disk segments as congested\. Requests to those segments are delayed between the specified minimum and maximum delay time for the duration of the simulation\.

**Syntax**

```
1. SELECT aurora_inject_disk_congestion(
2.    percentage_of_failure, 
3.    index, 
4.    is_disk, 
5.    time_interval, 
6.    minimum, 
7.    maximum
8. );
```Options

This fault injection query takes the following parameters:

*percentage\_of\_failure*  
The percentage of the disk to mark as congested during the failure event\. This is a double value between 0 and 100\. If you specify 0, then none of the disk is marked as congested\. If you specify 100, then the entire disk is marked as congested\.

*index*  
A specific logical block of data or storage node to use to simulate the failure event\.  
If you exceed the range of available logical blocks or storage nodes of data, you receive an error that tells you the maximum index value that you can specify\. To avoid this error, see [Displaying volume status for an Aurora PostgreSQL DB cluster](AuroraPostgreSQL.Managing.VolumeStatus.md)\.

*is\_disk*  
Indicates whether the injection failure is to a logical block or a storage node\. Specifying true means injection failures are to a logical block\. Specifying false means injection failures are to a storage node\.

*time\_interval*  
The amount of time to simulate the Aurora Replica failure\. The interval is in seconds\. For example, if the value is 20, the simulation runs for 20 seconds\.

*minimum, maximum*  
The minimum and maximum amount of congestion delay, in milliseconds\. Valid values range from 0\.0 to 100\.0 milliseconds\. Disk segments marked as congested are delayed for a random amount of time within the minimum and maximum range for the duration of the simulation\. The maximum value must be greater than the minimum value\.