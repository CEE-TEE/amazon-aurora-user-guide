# Adding options to Oracle DB instances<a name="Appendix.Oracle.Options"></a>

In Amazon RDS, an option is an additional feature\. Following, you can find a description of options that you can add to Amazon RDS instances running the Oracle DB engine\. To enable these options, you add them to an option group, and then associate the option group with your DB instance\. For more information, see [Working with option groups](USER_WorkingWithOptionGroups.md)\. 

Some options require additional memory to run on your DB instance\. For example, Oracle Enterprise Manager Database Control uses about 300 MB of RAM\. If you enable this option for a small DB instance, you might encounter performance problems due to memory constraints\. You can adjust the Oracle parameters so that the database requires less RAM\. Alternatively, you can scale up to a larger DB instance\. 

You can add the following options for Oracle DB instances\. 


****  

| Option | Option ID | 
| --- | --- | 
|  [Amazon S3 integration](oracle-s3-integration.md)  |  `S3_INTEGRATION`  | 
|  [Oracle Application Express \(APEX\)](Appendix.Oracle.Options.APEX.md)  |  `APEX` `APEX-DEV`  | 
|  [Oracle Enterprise Manager](Oracle.Options.OEM.md)  |  `OEM` `OEM_AGENT`  | 
|  [Oracle Java virtual machine](oracle-options-java.md)  |  `JVM`  | 
|  [Oracle Label Security](Oracle.Options.OLS.md)  |  `OLS`  | 
|  [Oracle Locator](Oracle.Options.Locator.md)  |  `LOCATOR`  | 
|  [Oracle Multimedia](Oracle.Options.Multimedia.md)  |  `MULTIMEDIA`  | 
|  [Oracle native network encryption](Appendix.Oracle.Options.NetworkEncryption.md)  |  `NATIVE_NETWORK_ENCRYPTION`  | 
|  [Oracle OLAP](Oracle.Options.OLAP.md)  |  `OLAP`  | 
|  [Oracle Secure Sockets Layer](Appendix.Oracle.Options.SSL.md)  |  `SSL`  | 
|  [Oracle Spatial](Oracle.Options.Spatial.md)  |  `SPATIAL`  | 
|  [Oracle SQLT](Oracle.Options.SQLT.md)  |  `SQLT`  | 
|  [Oracle Statspack](Appendix.Oracle.Options.Statspack.md)  |  `STATSPACK`  | 
|  [Oracle time zone](Appendix.Oracle.Options.Timezone.md)  |  `Timezone`  | 
|  [Oracle Transparent Data Encryption](Appendix.Oracle.Options.AdvSecurity.md)  |  `TDE`  | 
|  [Oracle UTL\_MAIL](Oracle.Options.UTLMAIL.md)  |  `UTL_MAIL`  | 
|  [Oracle XML DB](Appendix.Oracle.Options.XMLDB.md)  |  `XMLDB`  | 