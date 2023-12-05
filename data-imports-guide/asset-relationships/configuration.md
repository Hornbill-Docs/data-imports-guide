# Configuration

## Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

We have provided a default configuration file that can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument.
:::

## Example Configuration

```json
{
    "KeySafeKeyID": 0,
    "Database": {
        "Source": "mssql",
        "Authentication": "SQL",
        "Encrypt": false
    },
    "Query":"SELECT d.h_entity_l_id AS lid, al.h_name AS lname, d.h_entity_r_id AS rid, ar.h_name AS rname, d.h_dependency AS dep, i.h_impact AS imp FROM h_cmdb_config_items_dependency d LEFT JOIN h_cmdb_assets al ON d.h_entity_l_id = al.h_pk_asset_id LEFT JOIN h_cmdb_assets ar ON d.h_entity_r_id = ar.h_pk_asset_id LEFT JOIN h_cmdb_config_items_impact i ON d.h_entity_l_id = i.h_entity_l_id AND d.h_entity_r_id = i.h_entity_r_id",
    "AssetIdentifier": {
        "Parent": "lname",
        "Child": "rname",
        "Dependency": "dep",
        "Impact":"imp",
        "Hornbill": "Name"
    },
    "DepencencyMapping": {
        "SourceDependency":"HornbillDependency",
        "Runs":"Runs",
        "Runs On":"Runs On",
        "Hosts":"Hosts",
        "Hosted On":"Hosted On",
        "Members":"Members",
        "Member Of":"Member Of"
    },
    "ImpactMapping": {
        "SourceImpact":"HornbillImpact",
        "Low":"Low",
        "Medium":"Medium",
        "High":"High"
    },
    "RemoveLinks": false,
    "RemoveQuery":"SELECT d.h_entity_l_id AS lid, al.h_name AS lname, d.h_entity_r_id AS rid, ar.h_name AS rname, d.h_dependency AS dep, i.h_impact AS imp FROM h_cmdb_config_items_dependency d LEFT JOIN h_cmdb_assets al ON d.h_entity_l_id = al.h_pk_asset_id LEFT JOIN h_cmdb_assets ar ON d.h_entity_r_id = ar.h_pk_asset_id LEFT JOIN h_cmdb_config_items_impact i ON d.h_entity_l_id = i.h_entity_l_id AND d.h_entity_r_id = i.h_entity_r_id",
    "RemoveAssetIdentifier": {
        "Parent": "lname",
        "Child": "rname",
        "Dependency": "dep",
        "Impact":"imp",
        "Hornbill": "Name",
        "RemoveBothSides": true
    }
}
```
### Configuration Explanation

#### Hornbill Instance Specific Configuration Properties

- `KeySafeKeyID` - Type: `integer` - The ID (primary key) of the KeySafe Key that contains your database connection authentication information. Note, this KeySafe Key should be of type [Database Authentication](/data-imports-guide/asset-relationships/authentication#key-type-database-authentication).

#### SourceConfig

- `Source` - Type: `string` - The data source that the tool will connect to. One of:
    - `mssql` - Microsoft SQL Server (2005 or above).
    - `mysql` - MySQL Server 4.1+, MariaDB.
    - `mysql320` - MySQL Server v3.2.0 to v4.0.
    - `odbc` - ODBC driver (ODBC Data Source using SQL Server Driver).
    - `swsql` - Supportworks SQL (Core Services v3.x).
- `Authentication` - Type: `string` -  The type of authentication to use to connect to the SQL server. Can be either:
    - `Windows` - Windows Account authentication, uses the logged-in Windows account to authenticate, and not the authentication details in the KeySafe Key.
    - `SQL` - uses SQL Server authentication, and will use the details in the KeySafe Key defined to connect.
- `Encrypt` - Type: `boolean` - Used to specify whether the connection between the script and the database should be encrypted. **NOTE** There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to `true` if your SQL Server has been patched accordingly.
- `Query` - Type: `string` - The basic SQL query to retrieve asset information from the data source.
- `AssetIdentifier` - Type: `object` - a number of properties containing details to match asset information returned from the `Query`, above, to existing asset records in your Hornbill instance:
    - `Parent` - Type: `string` - specifies the column from the above `Query` that holds the Parent asset unique identifier.
    - `Child` - Type: `string` - specifies the column from the above `Query` that holds the Child asset unique identifier.
    - `Dependency` - Type: `string` - specifies the column from the above `Query` that holds the value of the Dependency.
    - `Impact` - Type: `string` - specifies the column from the above `Query` that holds the value of the Impact.
    - `Hornbill` - Type: `string` - specifies which column to use from the Hornbill asset records to match with the Parent and Child column output from the `Query`. The following values are supported:
        - `Name` - This will attempt to match the Hornbill asset using the Name field.
        - `Tag` - This will attempt to match the Hornbill asset using the Asset Tag field.
        - `Description` - This will attempt to match the Hornbill asset using the Description field.
- `DependencyMapping` - Type: `object` - A collection of key-value property pairs, to match the dependency column output from the `Query` to the available Hornbill dependency values. The property names should be the dependencies as expected from the `Query` output, and their values should be the matching depencency from your Hornbill instance.
- `ImpactMapping` - Type: `object` - A collection of key-value property pairs, to match the impact column output from the Query to the available Hornbill impact values. The property names should be the impacts as expected from the Query output, and their values should be the matching impact from your Hornbill instance.
- `RemoveLinks` - Type: `boolean` - Defines whether or not to attempt removal of asset relationship records.
- `RemoveQuery` - Type: `string` - The basic SQL query to retrieve records for asset relationship removal from the data source.
- `RemoveAssetIdentifier` - Type: `object` - A collection of propertied to define details to match asset information returned from the `RemoveQuery` resultset, above, to existing asset and relationship records in your Hornbill instance:
    - `Parent` - Type: `string` - specifies the column from the above `RemoveQuery` that holds the Parent asset unique identifier.
    - `Child` - Type: `string` - specifies the column from the above `RemoveQuery` that holds the Child asset unique identifier.
    - `Dependency` - Type: `string` - specifies the column from the above `RemoveQuery` that holds the value of the Dependency.
    - `Impact` - Type: `string` - specifies the column from the above `RemoveQuery` that holds the value of the Impact.
    - `Hornbill` - Type: `string` - specifies which column to use from the Hornbill asset records to match with the 'Parent' and 'Child' column output from the `RemoveQuery`. The following values are supported:
        - `Name` - This will attempt to match the Hornbill asset using the Name field.
        - `Tag` - This will attempt to match the Hornbill asset using the Asset Tag field.
        - `Description` - This will attempt to match the Hornbill asset using the Description field.
    - `RemoveBothSides` - Type: `boolean` - Defines whether the links on both sides of the relationship need to be removed.