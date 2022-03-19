# Aurora MySQL database engine updates: 2016\-10\-26 \(version 1\.8\.1\) \(deprecated\)<a name="AuroraMySQL.Updates.20161026"></a>

**Version:** 1\.8\.1

## Improvements<a name="AuroraMySQL.Updates.20161026.Improvements"></a>
+ Fixed an issue where bulk inserts that use triggers that invoke AWS Lambda procedures fail\.
+ Fixed an issue where catalog migration fails when autocommit is off globally\.
+ Resolved a connection failure to Aurora when using SSL and improved Diffie\-Hellman group to deal with LogJam attacks\.

## Integration of MySQL bug fixes<a name="AuroraMySQL.Updates.20161026.BugFixes"></a>
+ OpenSSL changed the Diffie\-Hellman key length parameters due to the LogJam issue\. \(Bug \#18367167\)