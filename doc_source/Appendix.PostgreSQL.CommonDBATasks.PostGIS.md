# Working with the PostGIS extension<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS"></a>

PostGIS is an extension to PostgreSQL for storing and managing spatial information\. If you are not familiar with PostGIS, see [PostGIS\.net](https://postgis.net/)\. 

You need to perform some setup before you can use the PostGIS extension\. The following list shows what you need to do\. Each step is described in greater detail later in this section\.

**Topics**
+ [Step 1: Connect to the DB instance using the user name used to create the DB instance](#Appendix.PostgreSQL.CommonDBATasks.PostGIS.Connect)
+ [Step 2: Load the PostGIS extensions](#Appendix.PostgreSQL.CommonDBATasks.PostGIS.LoadExtensions)
+ [Step 3: Transfer ownership of the extensions to the rds\_superuser role](#Appendix.PostgreSQL.CommonDBATasks.PostGIS.TransferOwnership)
+ [Step 4: Transfer ownership of the objects to the rds\_superuser role](#Appendix.PostgreSQL.CommonDBATasks.PostGIS.TransferObjects)
+ [Step 5: Test the extensions](#Appendix.PostgreSQL.CommonDBATasks.PostGIS.Test)
+ [PostGIS extension versions](#CHAP_PostgreSQL.Extensions.PostGIS)

## Step 1: Connect to the DB instance using the user name used to create the DB instance<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS.Connect"></a>

First, you connect to the DB instance using the user name that was used to create the DB instance\. That name is automatically assigned the `rds_superuser` role\. You need the `rds_superuser` role that is needed to do the remaining steps\.

The following example uses `SELECT` to show you the current user\. In this case, the current user should be the user name you chose when creating the DB instance\.

```
SELECT CURRENT_USER;
 current_user
 -------------
  myawsuser
 (1 row)
```

## Step 2: Load the PostGIS extensions<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS.LoadExtensions"></a>

Use `CREATE EXTENSION` statements to load the PostGIS extensions\. You must also load the `` extension\. You can then use the `\dn` command to list the owners of the PostGIS schemas\.

```
CREATE EXTENSION postgis;
CREATE EXTENSION fuzzystrmatch;
CREATE EXTENSION postgis_tiger_geocoder;
CREATE EXTENSION postgis_topology;
\dn
     List of schemas
     Name     |   Owner
--------------+-----------
 public       | myawsuser
 tiger        | rdsadmin
 tiger_data   | rdsadmin
 topology     | rdsadmin
(4 rows)
```

## Step 3: Transfer ownership of the extensions to the rds\_superuser role<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS.TransferOwnership"></a>

Use the ALTER SCHEMA statements to transfer ownership of the schemas to the `rds_superuser` role\.

```
ALTER SCHEMA tiger OWNER TO rds_superuser;
ALTER SCHEMA tiger_data OWNER TO rds_superuser;
ALTER SCHEMA topology OWNER TO rds_superuser;
\dn
       List of schemas
     Name     |     Owner
--------------+---------------
 public       | myawsuser
 tiger        | rds_superuser
 tiger_data   | rds_superuser
 topology     | rds_superuser
(4 rows)
```

## Step 4: Transfer ownership of the objects to the rds\_superuser role<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS.TransferObjects"></a>

Use the following function to transfer ownership of the PostGIS objects to the `rds_superuser` role\. Run the following statement from the psql prompt to create the function\.

```
CREATE FUNCTION exec(text) returns text language plpgsql volatile AS $f$ BEGIN EXECUTE $1; RETURN $1; END; $f$;
```

Next, run this query to run the exec function that in turn runs the statements and alters the permissions\.

```
SELECT exec('ALTER TABLE ' || quote_ident(s.nspname) || '.' || quote_ident(s.relname) || ' OWNER TO rds_superuser;')
  FROM (
    SELECT nspname, relname
    FROM pg_class c JOIN pg_namespace n ON (c.relnamespace = n.oid) 
    WHERE nspname in ('tiger','topology') AND
    relkind IN ('r','S','v') ORDER BY relkind = 'S')
s;
```

## Step 5: Test the extensions<a name="Appendix.PostgreSQL.CommonDBATasks.PostGIS.Test"></a>

Add `tiger` to your search path using the following command\.

```
SET search_path=public,tiger;
```

Test `tiger` by using the following SELECT statement\.

```
SELECT na.address, na.streetname, na.streettypeabbrev, na.zip
FROM normalize_address('1 Devonshire Place, Boston, MA 02109') AS na;
address | streetname | streettypeabbrev |  zip
---------+------------+------------------+-------
       1 | Devonshire | Pl               | 02109
(1 row)
```

Test `topology` by using the following SELECT statement\.

```
SELECT topology.createtopology('my_new_topo',26986,0.5);
 createtopology
----------------
              1
(1 row)
```

## PostGIS extension versions<a name="CHAP_PostgreSQL.Extensions.PostGIS"></a>

The following table shows the PostGIS versions that ship with the RDS for PostgreSQL versions\.


| PostgreSQL version | PostGIS version | 
| --- | --- | 
| 13\.2, 13\.1 | 3\.0\.2 | 
| 12\.6 | 3\.0\.2 | 
| 12\.5, 12\.4, 12\.3, 12\.2 | 3\.0\.0 | 
| 11\.11, 11\.10, 11\.9, 11\.8, 11\.7, 11\.6, 11\.5 | 2\.5\.2 | 
| 11\.4, 11\.2, 11\.1 | 2\.5\.1 | 
| 10\.16, 10\.15, 10\.14, 10\.13, 10\.12, 10\.11, 10\.10 | 2\.5\.2 | 
| 10\.9, 10\.7, 10\.6\. 10\.5, 10\.4 | 2\.4\.4 | 
| 10\.3, 10\.1 | 2\.4\.2 | 
| 9\.6\.21, 9\.6\.20, 9\.6\.19, 9\.6\.18, 9\.6\.17, 9\.6\.16, 9\.6\.15 | 2\.5\.2 | 
| 9\.6\.14, 9\.6\.12, 9\.6\.11, 9\.6\.10, 9\.6\.9 | 2\.3\.7 | 
| 9\.6\.8, 9\.6\.6 | 2\.3\.4 | 
| 9\.6\.5, 9\.6\.3, 9\.6\.2 | 2\.3\.2 | 
| 9\.6\.1 | 2\.3\.0 | 
| 9\.5\.25, 9\.5\.24, 9\.5\.23, 9\.5\.22, 9\.5\.21, 9\.5\.20, 9\.5\.19 | 2\.5\.2 | 
| 9\.5\.18, 9\.5\.16, 9\.5\.15, 9\.5\.14, 9\.5\.13, 9\.5\.12, 9\.5\.10, 9\.5\.9, 9\.5\.7, 9\.5\.6 | 2\.2\.5 | 
| 9\.5\.4, 9\.5\.2 | 2\.2\.2 | 

**Note**  
PostgreSQL 10\.5 added support for the `libprotobuf` extension version 1\.3\.0 to the PostGIS component\. 