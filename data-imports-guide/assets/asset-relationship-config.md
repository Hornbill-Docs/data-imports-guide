# Configuration

## Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

We have provided a selection of default configuration files, one for each of the systems that Hornbill customers most often import asset data from, in each of the downloadable archives. This can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument
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

- `KeySafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your database authentication details. Set to 0 for importing directly from CSV files. See the [KeySafe section of the Authentication article](/data-imports-guide/assets/authentication#keysafe) for more information on supported key types.

#### SourceConfig

- `Source` - Type: `string` - The data source that the tool will connect to. One of:
  - `mssql` - Microsoft SQL Server (2005 or above) - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `mysql` - MySQL Server 4.1+, MariaDB - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `mysql320` - MySQL Server v3.2.0 to v4.0 - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `odbc` - ODBC driver (ODBC Data Source using SQL Server Driver)- will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication) When using ODBC as a data source, the `Database`, `UserName`, `Password` and `Query` parameters should be populated accordingly:
    - Database - this should be populated with the name of the ODBC connection on the PC that is running the tool
    - UserName - this should be the SQL authentication Username to connect to the Database
    - Password - this should be the password for the above username
  - `swsql` - Supportworks SQL (Core Services v3.x) - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
- `Database` - Type: `object` - Only in use if `Source` is set to `mssql`, `mysql`, `mysql320` or `swsql`
    - `Authentication` - Type: `string` -  The type of authentication to use to connect to the SQL server. Can be either:
      - `Windows` - Windows Account authentication, uses the logged-in Windows account to authenticate, and not the authentication details in the KeySafe Key
      - `SQL` - uses SQL Server authentication, and will use the details in the KeySafe Key defined to connect. The Key Type should be [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
    - `Encrypt` - Type: `boolean` - Used to specify whether the connection between the script and the database should be encrypted. **NOTE** There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to `true` if your SQL Server has been patched accordingly.
- `Query` - Type: `string` - The basic SQL query to retrieve asset information from the data source. See `AssetTypes` below for further information on filtering.
- `LDAP` - Type: `object` - Only in use if `Source` is set to `ldap`
    - `Server` - Type: `object` - LDAP host specific configuration:
      - `InsecureSkipVerify` - Type: `boolean` - Used in conjunction with SSL or TLS connection types and allows the verification of SSL Certifications to be disabled i.e. `ON` sets the InsecureSkipVerify variable when querying the LDAP to `true`.
      - `Debug` - Type: `boolean` - Enables LDAP Connection Debugging. This should only ever be enabled to troubleshoot connection issues during the initial setup and testing.
      - `ConnectionType` - Type: `string` - The type of HTTP connection to use when communicating with the directory Server. Normal HTTP (leave the value blank), `SSL`, and `TLS` are supported.
    - `Query` - Type: `object` - LDAP query specific configuration:
      - `Attributes` - Type: `array` - A list of LDAP attributes to return as part of the query
      - `Scope` - Type: `integer` - Search Scope (ScopeBaseObject = 0, ScopeSingleLevel = 1, ScopeWholeSubtree = 2) Default is 1.
      - `DerefAliases` - Type: `integer` - Dereference Aliases (NeverDerefAliases = 0, DerefInSearching = 1, DerefFindingBaseObj = 2, DerefAlways = 3), allows you to choose at what stage during the search operation (LDAP Query) aliases are dereferenced. The default value (1) should be suitable for most.
      - `TypesOnly` - Type: `boolean` - Enabling this setting will cause the query to return attribute types (descriptions) rather than attribute values.
      - `SizeLimit` - Type: `integer` - Allows you to set a size limit in relation to the result set that can be returned. Setting to '0' disables this setting.
      - `TimeLimit` - Type: `integer` - Allows you to impose a time limit (seconds) in relation to how long the LDAP query can run before timing out. Setting to '0' disables this setting.
- `Google` - Type: `object` - Only in use if `Source` is set to `google`
    - `Customer` - Type: `string` - The Customer ID of the Google Account. Supports my_customer to return devices enrolled to the deafult account that the KeySafe key was created with
    - `Query` - Type: `string` - Search string as per the [Google API Documentation](https://developers.google.com/admin-sdk/directory/reference/rest/v1/chromeosdevices/list)
    - `OrgUnitPath` - Type: `string` - The full path of the Google organizational Unit where the devices reside
- `Certero` - Type: `object` - Only in use if `Source` is set to `certero`
    - `Expand` - Type: `string` - The Certero oData query which defines the columns and associated entitiy records that can be mapped into Hornbill asset records
- `AssetIdentifier` - an object containing details to match asset information returned from the `Query`, above, to existing asset records in your Hornbill instance:
    - `Paernt` - specifies the column from the above `Query` that holds the Parent asset unique identifier
    - `Child` - specifies the column from the above Query that holds the Child asset unique identifier
    - `Dependency` - specifies the column from the above Query that holds the value of the Dependency
    - `Impact` - specifies the column from the above Query that holds the value of the Impact
    - `Hornbill` - specifies which column to use from the Hornbill asset records to match with the Parent and Child column output from the `Query`. The following values are supported:
        - `Name` - This will attempt to match the Hornbill asset using the Name field
        - `Tag` - This will attempt to match the Hornbill asset using the Asset Tag field
        - `Description` - This will attempt to match the Hornbill asset using the Description field
- `DependencyMapping` - an object containing properties to match the dependency column output from the `Query` to the available Hornbill dependency values. The property names should be the dependencies as expected from the `Query` output, and their values should be the matching depencency from your Hornbill instance
- `ImpactMapping`- an object containing properties to match the impact column output from the Query to the available Hornbill impact values. The property names should be the impacts as expected from the Query output, and their values should be the matching impact from your Hornbill instance
- `RemoveLinks` - Boolean true or flalse, defines whether or not to attempt removal of asset relationship records
- `RemoveQuery` - The basic SQL query to retrieve records for asset relationship removal from the data source
- `RemoveAssetIdentifier` - an object containing details to match asset information returned from the 'RemovalQuery', above, to existing asset and relationship records in your Hornbill instance:
    - `Parent` - specifies the column from the above 'RemovalQuery' that holds the Parent asset unique identifier
    - `Child` - specifies the column from the above 'RemovalQuery' that holds the Child asset unique identifier
    - `Dependency` - specifies the column from the above 'RemovalQuery' that holds the value of the Dependency
    - `Impact` - specifies the column from the above 'RemovalQuery' that holds the value of the Impact
    - `Hornbill` - specifies which column to use from the Hornbill asset records to match with the 'Parent' and 'Child' column output from the 'RemovalQuery'. The following values are supported:
        - `Name` - This will attempt to match the Hornbill asset using the Name field
        - `Tag` - This will attempt to match the Hornbill asset using the Asset Tag field
        - `Description` - This will attempt to match the Hornbill asset using the Description field
    - `RemoveBothSides` - Boolean true or false, if the links on both sides of the relationship need to be removed

## API Key Rules

This utility uses ([API keys](https://docs.hornbill.com/esp-fundamentals/security/api-keys)):
- data:entityAddRecord
- data:entityDeleteRecord
- data:entityUpdateRecord
- data:getRecordCount
- data:queryExec
- system:logMessage
- apps/com.hornbill.servicemanager/Asset:linkAsset
- apps/com.hornbill.servicemanager/Asset:unlinkAsset

## Execute
### Command Line Parameters
- file - Defaults to 'conf.json' - Name of the Configuration file to load.
- dryrun - Defaults to 'false' - Set to true and the XMLMC for the creation and update of assets will not be called, and instead the generated XML for each asset will be dumped to the log file. This is to aid in debugging the initial connection information.
- creds - Defaults to 'false' - Set to true to return the API key that is stored in your import configuration. See NOTE in First Run, below. As well as user & computer specific access, you will also be prompted to input the ID of the instance that the API key was generated on.
- version - Defaults to 'false' - Set to true to return the version number of the tool, then exit

#### **First Run**

From version **2.0.0** of this utility, when you first run the utility it will prompt you for the ID of your Hornbill instance (case-sensitive), and the API key (also case-sensitive) that will be used to authenticate the API calls back into Hornbill. This information will be encrypted, and stored locally on the client PC that will be running the tool. For each subsequent import run, the utility will decrypt your instance ID and API key, and will use those to make the API calls back into Hornbill.

NOTE - the encrypted instance ID and API key can only be decrypted on the computer, and by the user, that performed the encryption, so please keep this in mind when scheduling your imports.

Should you wish to use a different API key to what has been previously encrypted, just delete the **import.cfg** file from the folder where the import binary resides, and re-run your import from the command line inputting the Instance ID and new API Key as you would have on its first run.

## Testing

If you run the application with the argument dryrun=true then no asset relationships will be created or updated, the XML used to create or update will be saved in the log file so you can ensure the data mappings are correct before running the import.

``asset_rel_import.exe -dryrun=true``

## Scheduling
### Windows

You can schedule asset_rel_import.exe to run with any optional command line argument from Windows Task Scheduler:

- Ensure the user account running the task has rights to asset_rel_import.exe and the containing folder.
- Make sure the Start In parameter contains the folder where asset_rel_import.exe resides in otherwise it will not be able to pick up the correct path.

## Logging

ogging output is saved in the log directory in the same directory as the executable the file name contains the date and time the import was run ``asset_rel_log_20190925140000``.log 