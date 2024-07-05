# Configuration

## Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

We have provided a selection of default configuration files, one for each of the systems that Hornbill customers most often import asset data from, in each of the downloadable archives. This can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument.
:::

## Example Configuration

```json
{
  "KeySafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "mssql",
    "Database": {
      "Authentication": "SQL",
      "Encrypt": false,
      "Query": ""
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "Query": "",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "InPolicy": "yes",
      "AdditionalFilters": [
        {
          "Field": "osVersion",
          "Operator": "REGEXMATCH",
          "Value": "^[0-9]{1,}\\.[0-9]{1,}\\.[0-9]{1,}\\.[0-9]{1,}$"
        },
        {
          "Field": "imei",
          "Operator": "EMPTY"
        }
      ],
      "AssetIdentifier": {
        "SourceColumn": "SystemSerialNumber",
        "SourceKey": "SourceDataPrimaryKey",
        "Entity": "AssetsComputer",
        "EntityColumn": "h_serial_number",
        "SourceContractColumn": "Contract",
        "SourceSupplierColumn": "Supplier",
        "SharedWith": {
          "AllowRemovals": true,
          "UserIDField": "SharedWithSourceUserIDField",
          "Query": ""
        }
      }
    },
    {
      "AssetType": "Laptop",
      "OperationType": "Both",
      "Query": "",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      }
    },
    {
      "AssetType": "Desktop",
      "OperationType": "Both",
      "Query": "",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      }
    },
    {
      "AssetType": "Virtual Machine",
      "OperationType": "Both",
      "Query": "",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "",
    "h_site": "",
    "h_asset_tag": "",
    "h_acq_method": "",
    "h_actual_retired_date": "",
    "h_beneficiary": "",
    "h_building": "",
    "h_cost": "",
    "h_cost_center": "",
    "h_country": "",
    "h_created_date": "",
    "h_deprec_method": "",
    "h_deprec_start": "",
    "h_description": "",
    "h_disposal_price": "",
    "h_disposal_reason": "",
    "h_floor": "",
    "h_geo_location": "",
    "h_invoice_number": "",
    "h_location": "",
    "h_location_type": "",
    "h_maintenance_cost": "",
    "h_maintenance_ref": "",
    "h_notes": "",
    "h_operational_state": "",
    "h_order_date": "",
    "h_order_number": "",
    "h_owned_by": "",
    "h_owned_by_name": "",
    "h_product_id": "",
    "h_received_date": "",
    "h_residual_value": "",
    "h_room": "",
    "h_scheduled_retire_date": "",
    "h_supplier_id": "",
    "h_supported_by": "",
    "h_used_by": "",
    "h_used_by_name": "",
    "h_version": "",
    "h_warranty_expires": "",
    "h_warranty_start": ""
  },
  "AssetTypeFieldMapping": {
    "h_name": "",
    "h_mac_address": "",
    "h_net_ip_address": "",
    "h_net_computer_name": "",
    "h_net_win_domain": "",
    "h_model": "",
    "h_manufacturer": "",
    "h_cpu_info": "",
    "h_description": "",
    "h_last_logged_on": "",
    "h_last_logged_on_user": "",
    "h_memory_info": "",
    "h_net_win_dom_role": "",
    "h_optical_drive": "",
    "h_os_description": "",
    "h_os_registered_to": "",
    "h_os_serial_number": "",
    "h_os_service_pack": "",
    "h_os_type": "",
    "h_os_version": "",
    "h_physical_disk_size": "",
    "h_serial_number": "",
    "h_cpu_clock_speed": "",
    "h_physical_cpus": "",
    "h_logical_cpus": "",
    "h_bios_name": "",
    "h_bios_manufacturer": "",
    "h_bios_serial_number": "",
    "h_bios_release_date": "",
    "h_bios_version": "",
    "h_max_memory_capacity": "",
    "h_number_memory_slots": "",
    "h_net_name": "",
    "h_subnet_mask": ""
  }
}
```

### Configuration Explanation

#### Hornbill Instance Specific Configuration Properties

- `KeySafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your data source authentication details. Set to 0 for importing directly from CSV files. See the [KeySafe section of the Authentication article](/data-imports-guide/assets/authentication#keysafe) for more information on supported key types.
:::tip
The KeySafe Key ID is the unique identifier of the key, and can be found in the URL when you are on the key details form in your browser. In the example below, `4` is the KeySafe Key ID:

`https://live.hornbill.com/yourinstanceid/admin/platform/security/keysafe/4/`
:::
- `LogSizeBytes` - Type: `integer` - The maximum size that the generated Log Files should be in bytes. Setting this value to 0 will cause the tool to create one log file and not split the results between multiple logs.
- `HornbillUserIDColumn` - Type: `string` - Used to specify the Hornbill User ID column for matching users against (asset owners, used by etc). Supported values: `h_user_id` (default), `h_employee_id`, `h_email`, `h_name`, `h_attrib1`, `h_attrib8` and `h_login_id`. **Please note:** `last logged on`, `owned by` and `used by` will use the same field - i.e. one can NOT specify which column to match to individually.

#### SourceConfig

- `Source` - Type: `string` - The data source that the tool will connect to. One of:
  - `mssql` - Microsoft SQL Server (2005 or above) - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `mysql` - MySQL Server - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `mysql320` - MySQL Server v3.2.0 to v4.0 - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `odbc` - ODBC driver - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `swsql` - Supportworks SQL (Core Services v3.x) - will use KeySafe key type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication)
  - `csv` - CSV / Text file(s) - doesn't require KeySafe keys
  - `nexthink` - Nexthink - will use KeySafe key type [Username + Password](/data-imports-guide/assets/authentication#key-type-username-password)
  - `ldap` - LDAP / Active Directory - will use KeySafe key type [LDAP Authentication](/data-imports-guide/assets/authentication#key-type-ldap)
  - `google` - Google Workspace Enterprise Chrome OS - will use KeySafe key type [Google Workspace](/data-imports-guide/assets/authentication#key-type-google-workspace)
  - `certero` - Certero - will use KeySafe key type [Certero](/data-imports-guide/assets/authentication#key-type-certero)
  - `workspaceone` - vmware Workspace One UEM - will use KeySafe type [VMWare Workspace One UEM](/data-imports-guide/assets/authentication#key-type-vmware-workspace-one-uem)
  - `intune` - Microsoft Intune - will use KeySafe type [Microsoft Intune](/data-imports-guide/assets/authentication#key-type-microsoft-intune)
  - `cynerio` - Cynerio - will use KeySafe type [Cynerio](/data-imports-guide/assets/authentication#key-type-cynerio)
  - `azureresourcequery` - Azure Resource Query - will use KeySafe type [Azure Resource Query](/data-imports-guide/assets/authentication#key-type-azure-resource-query)
- `CSV` - Type: `object` - Only in use if `Source` is set to `csv`
    - `CarriageReturnRemoval` - Type: `boolean` - Certain CSV exporting systems will add extra carriage returns as a record delimiter. This is expected not to be common, hence the setting is left out of the configuration files (it is added to conf_computerSyste      * `json only for completeness sake). If not set, then the default value is `false` and no carriages returns will be stripped from the data. If set to `true`, then all carriage returns (possibly even intended ones) will be stripped.
    - `CommaCharacter` - Type: `string` - The field separator (single) character - if left out, the default character will be a comma.
    - `FieldsPerRecord` - Type: `integer` - The ability to give the CSV reader a hint about the number of fields in each record. Defaults to `0`, leaving the CSV reader to do the heavy lifting.
    - `LazyQuotes` - Type: `boolean` - The ability to give the CSV reader a hint that the csv file might be using lazy quotes. Defaults to `false`.
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
- `Intune` - Type: `object` - Only in use if `Source` is set to `intune`
    - `Fields` - Type: `array` - A list of fields that should be returned during the API calls to Intune. These are the fields that can be mapped in the field mappings, below. See the [Intune documentation](https://learn.microsoft.com/en-us/graph/api/resources/intune-devices-manageddevice?view=graph-rest-1.0#properties) for detailed information regarding field availability
    - `ImportDetectedApps` - Type: `boolean` - Defaults to `false`. If set to `true`, then all detected apps against the Intune Managed Devices will be imported as Software Inventory records
- `Cynerio` - Type: `object` - Only in use if `Source` is set to `cynerio`
    - `Fields` - Type: `array` - A list of fields that should be returned during the API calls to Cynerio. These are the fields that can be mapped in the field mappings, below. Speak to your Cynerio administrator or representative for detailed information regarding field availability
- `AzureResourceQuery` - Type: `object` - Only in use if `Source` is set to `azureresourcequery`
    - `SubscriptionIDs` - Type: `array` - An array of strings, which can hold a list of Subscription IDs to apply to the Azure Resource Query.
    - `ManagementGroups` - Type: `array` - An array of strings, which can hold a list of Management Group IDs to apply to the Azure Resource Query. If both ManagementGroups and SubscriptionIDs are provided, ManagementGroups will be ignored as the Azure Resource Query API only supports one or the other.
    - `SoftwareKeysafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your data source authentication details. Azure Resource Query has no mechanism to return sotware inventory items, so the utility uses Azure Log Analytics and the Automation Update/Change/Inventory module to retrieve software installed on your Arc registered computers.
    - `SoftwareLogAnalyticsWorkspaceID` - Type: `string` - The ID (GUID) of the Log Analytics Workspace that contains the ConfigurationData table, that holds your asset software inventory records.

#### AssetTypes

An array of objects detailing the asset types to import. 

:::important
During the import process assets of each type as defined below are retrieved from Hornbill and cached locally in memory to aid in the matching of existing asset records for update. This is to improve the performance of the import process, as well as to significantly reduce load on your Hornbill instance when the imports are running. If you have imported asset records into one particular Hornbill asset type, then re-import the same asset records from the source into a different Hornbill asset type, then you may see duplicate asset records being created. We suggest you run the tool against your configuration in dryrun mode and check that no duplicates would be created, and if they are then you need to update your import configuration accordingly. See AssetType, below, for one such method of avoiding duplicates. 
:::

* `AssetType` - Type: `string` - the Asset Type Name which needs to match a correct Asset Type Name in your Hornbill Instance, OR if `OperationType` below is set to `Update`, and you wish to update assets of ALL types of a specific class, then you can populate this property with `__all__:computer`, where `computer` is the asset class to update. Valid classes are:
  * `basic`
  * `computer`
  * `computerPeripheral`
  * `dataProcessingRecord`
  * `mobileDevice`
  * `networkDevice`
  * `printer`
  * `software`
  * `system`
  * `telecoms`
* `OperationType` - Type: `string` - The type of operation that should be performed on discovered assets - can be `Create`, `Update` or `Both`. Defaults to `Both` if no value is provided
* `Clauses` - Type: `array` - An array of objects, containing zero or more query clauses that should be used in the Azure Resource Query API call. Only used when `SourceConfig > Source` is set to `azureresourcequery`. The clauses provided in this list will be joined with a pipe delimiter, and appended with an order by name, before being sent to the Azure Resource Graph Query API. See the [Resource Graph documentation](https://learn.microsoft.com/en-us/azure/governance/resource-graph/first-query-rest-api) and the [Kusto Query Language reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/kql-quick-reference) for more information
* `CSVFile` - Type: `string` - The path to the CSV file to use as the data source for this specific asset type. Only required when `SourceConfig > Source` is set to `CSV`
* `CSVFilters` - Type: `array` - An array of objects, containing zero or more filters that should be applied when working out which rows from the CSV file should be included in the asset type import. Only required when `SourceConfig > Source` is set to `CSV`, and this is non-mandatory. If no filters are provided, then all records will be imported from the CSV into the target asset type.
  * `Column` - Type: `string` - The column name from the CSV that the filter should apply to
  * `Operator` - Type: `string` - The operator that should be used when applying the filter. Valid values are:
    * `EQUALS` - Does the filter Value (below) exactly match the column value
    * `CONTAINS` - Is the filter Value (below) contained within the column value (wildcard type functionality, `*column_value*`)
    * `IN` - Does the column value have an exact match in an array of strings supplied in `Value`, below
  * `Value` - Type: `string` - The value to be applied in the filter
* `PreserveShared` - Type: `boolean` - If set to `true`, when updating assets that are Shared, then the Used By fields will not be updated. If the property is not provided, this defaults to `false`
* `PreserveState` - Type: `boolean` - If set to `true` then the State field will not be updated. If the property is not provided, this defaults to `false`
* `PreserveSubState` - Type: `boolean` - If set to `true` then the SubState fields will not be updated. If the property is not provided, this defaults to `false`
* `PreserveOperationalState` - Type: `boolean` - If set to `true` then the Operational State field will not be updated. If the property is not provided, this defaults to `false`
* `InPolicy` - Type: `string` - If set, the In Policy flag will be updated when processing assets of this type. Supports the following values:
  * `yes` - Sets the processed assets to In Policy
  * `__clear__` - Clears the In-Policy flag for the processed assets
* `NexthinkPlatform` - Type: `string` - When using Nexthink as a data source, this can be optionally set to one of the following values, depending on which type of asset records you wish to import:
  * `windows`
  * `mac_os`
  * `mobile`
* `LDAPDSN` - Type: `string` - The Distinguished Name of the LDAP container to execute your query in. Only required when `SourceConfig > Source` is set to `LDAP`
* `Query` - Type: `string` - Not used for Workspace One or direct CSV file imports. For the other data sources:
  * Additional SQL clauses to be appended to the Query from SourceConfig > Database > Query, to retrieve assets of that asset type. 
  * Certero oData filter for returning asset details for that asset type
  * Nexthink query for returning asset details for that asset type
  * LDAP query for returning asset details for that asset type
  * Microsoft Graph oData filter for returning asset details from Intune. See the [filter parameter documentation from Microsoft](https://learn.microsoft.com/en-us/graph/filter-query-parameter?tabs=http) for instruction of how these filters can be constructed, and the [managed device resource type documentation](https://learn.microsoft.com/en-us/graph/api/resources/intune-devices-manageddevice?view=graph-rest-1.0#properties) for a list of available properties.
* `Filter` - Type: `object` - Used exclusively for applying filters to Workspace One queries, all are optional:
  * `User` - Type: `string` - Username the device enrolled under
  * `ModelIdentifier` - Type: `string` - Partial search by device model. Search by MD20 would return device with model MD200LL
  * `DevicePlatformType` - Type: `string` - Device platform type, i.e. Apple, Android, WindowsPC, etc.
  * `Ownership` - Type: `string` - Device ownership type i.e. Corporate, Employee, Shared
  * `OrganizationGroupUUID` - Type: `string` - The UUID of the Organization Group that the devices are members of. Defaults to the keysafe key user's OrganizationGroup
  * `ComplianceStatus` - Type: `string` - The compliance status of the devices i.e. Compliant, NonCompliant etc
  * `SeenSince` - Type: `string` - Specifies the datetime filter for device search, which retrieves the devices that are seen after this datetime stamp.
* `CynerioFilters` - Type: `object` - A list of fields and values to use when filtering the Cynerio asset API call resultsets by, in the following format:
  * `"CynerioFieldName":"ValueToMatch"`
* `AdditionalFilters` - Type: `array` - A list of filter objects, to further filter the list of assets returned from `Cynerio`, `Intune` or `Google Workspace`. Each item in the list must match as `true` for the asset to be imported, and are defined as objects containing:
  * `Field` - Type: `string` - The data source field ID to perform the filter against.
  * `Operator` - Type: `string` - The operator to apply to the filter, can be one of:
    * `ISEMPTY` - Is the field empty (an empty string or NULL value)
    * `NOTEMPTY` - Does the field contain a value (so NOT an empty string or NULL value)
    * `EQUALS` - The field contains an exact match to the `Value` defined below (case insensitive)
    * `NOTEQUALS` - The field does not equal the `Value` defined below (case insensitive)
    * `CONTAINS` - The field contains the `Value` defined below (case insensitive)
    * `NOTCONTAINS` - The field does not the `Value` defined below (case insensitive)
    * `REGEXMATCH` - The field matches the Regular Expression in the `Value` defined below
  * `Value` - Used by the `EQUALS`, `NOTEQUALS`, `CONTAINS`, `NOTCONTAINS`, `REGEXMATCH` operators.
* `AssetIdentifier` - Type: `object` - An object containing details to help in the identification of existing asset records in the Hornbill instance. If value in an imported records DBColumn matches the value in the EntityColumn of an asset in Hornbill (within the defined Entity), then the asset record will be updated rather than a new asset being created:
  * `SourceColumn` - Type: `string` - Specifies the unique identifier column from the source data query. This can be either a column name, or a Go Template. **NOTE:** Importing from Certero requires this to be a Go template.
  * `SourceKey` - Type: `string` - The Primary Key field of the source asset records. Must be set when using the `SharedWith` feature, below.
  * `Entity` - Type: `string` - the Hornbill entity where data is stored, one of:
      * `Asset`
      * `AssetsComputer`
      * `AssetsComputerPeripheral`
      * `AssetsMobileDevice`
      * `AssetsNetworkDevice`
      * `AssetsPrinter`
      * `AssetsSoftware`
      * `AssetsTelecoms`
  * `EntityColumn` - Type: `string` - specifies the unique identifier column from the Hornbill entity specified above. Supported values per `Entity` type:
    * `Asset`:
      * `h_pk_asset_id`
      * `h_asset_tag`
      * `h_name`
      * `h_external_id`
      * `h_tag_id`
      * `h_tag_mac`  
    * `AssetsComputer`
      * `h_bios_serial_number`
      * `h_mac_address`
      * `h_net_computer_name`
      * `h_net_ip_address`
      * `h_net_name`
      * `h_net_win_dom_role`
      * `h_os_serial_number`
      * `h_serial_number`
      * `h_private_ip_address`
      * `h_public_ip_address`
      * `h_wmac_address`
    * `AssetsComputerPeripheral`
      * `h_serial_number`
    * `AssetsMobileDevice`
      * `h_data_number`
      * `h_device_id`
      * `h_device_name`
      * `h_imei_number`
      * `h_imsi_number`
      * `h_ip_address`
      * `h_mac_address`
      * `h_phone_number`
      * `h_serial_number`
      * `h_sim_number`
    * `AssetsNetworkDevice`
      * `h_mac_address`
      * `h_net_ip_address`
      * `h_serial_number`
    * `AssetsPrinter`
      * `h_net_ip_address`
      * `h_net_mac_address`
      * `h_wmac_address`
      * `h_serial_number`
    * `AssetsSoftware`
      * `h_license_key`
    * `AssetsTelecoms`
      * `h_phone_number`
      * `h_extension`
      * `h_ip_address`
      * `h_mac_address`
      * `h_wmac_address`
      * `h_serial_number`
  * `SourceContractColumn` - Type: `string` - Specifies the unique identifier column from the database query - the data in this column of the query result should be the Contract ID within Hornbill (ie `C20200700001` from `/suppliermanager/suppliercontract/view/C20200700001/`; `'C20200700001' AS Contract` hardcoded in the SQL query)
  * `SourceSupplierColumn` - Type: `string` - Specifies the unique identifier column from the database query - the data in this column of the query result should be the Supplier ID within Hornbill (ie `###` from `suppliermanager/supplier/view/###/`)
  * `SharedWith` - Type: `object` - Contains the configuration information that allows assets being imported from `Database` type data sources to be set as shared usage between one or more users:
    * `AllowRemovals` - Type: `boolean` - When set to `true`, the utility will remove any users from the Shares With section of the asset being updated, when they are not listed in the source asset record. When set to `false`, the utility will NOT remove any shared used from the asset being updated. 
    * `UserIDField` - Type: `string` - The name of the field that contains the User ID values from the `Query` below.
    * `ShareType` - Type: `string` - The type of shares to manage. Can be one of:
      * `All Contacts`
      * `Company`
      * `Contact`
      * `Customer Organization`
      * `Division`
      * `Team`
      * `User`
    * `Query` - Type: `string` - The Query to run against database data source, to return your users associated to your asset record. Note, to inject the source Asset ID into your query, use `{{AssetID}}` in the appropriate part of your query. For example:
      * `SELECT h_pk_asset_id, h_fk_userid FROM h_cmdb_assets_users WHERE h_fk_assetid = '{{AssetID}}'`
  * `SoftwareInventory` - Type: `object` - Details pertaining to the import of software inventory records for the specified asset type:
      * `AssetIDColumn` - Type: `string` - The column from the asset type query that contains its primary key. This can be either a column name, or a Go Template. **NOTE**: this is ignored when performing imports from **Workspace One**. 
      * `AppIDColumn` - Type: `string` - the column from the Software Inventory that holds the software unique ID. This can be either a column name, or a Go Template. **NOTE**: this is ignored when performing imports from **Certero, CSV, LDAP or Nexthink** - the App ID to match is a concatenation of the publisher, name and version fields (no spaces between them)
      * `Query` - Type: `string` - The query that will be run per asset, to return its software inventory records. `{{AssetID}}` - in the query will be replaced by each assets primary key value, whose column is defined in the `AssetIDColumn` property. **NOTE**: this is NOT being processed as a template (note the absence of the full stop). **ALSO NOTE**: This is not used when importing assets from **Certero, Arc or Workspace One**.
      * `Clauses` - Type: `array` - Only used whem importing assets from **Azure Resource Query/Arc**. A list of clauses to pass to the Log Analytics query API to return Software Inventory records for imported assets. `{{AssetID}}` - in the query will be replaced by each assets primary key value, whose column is defined in the `AssetIDColumn` property. **NOTE**: this is NOT being processed as a template (note the absence of the full stop).
      * `Mapping` - Type: `object` - A key-value pair list of mapping in the format `"Hornbill Column": "Mapped Value"`, maps data into the software inventory records

#### AssetGenericFieldMapping

Maps data in to the generic Asset record Any value templated with `{{.columnName}}` will be populated with the corresponding response from the data source records. Providing a value of `__clear__` will `NULL` that column for the record in the database when assets are being updated ONLY. This can either be hard-coded in the config, or sent as a string column within the SQL query resultset as so:

```sql
SELECT '__clear__' AS clearColumn
``` 

Then `[clearColumn]` can be used in the mapping, for example.

Any other value is treated as written examples below:

* `"h_name":"{{.MachineName}}"` - The value of MachineName is taken from the SQL output and populated within this field;
* `"h_description":"This is a description"` - The value of "h_description" would be populated with "This is a description" for ALL imported assets;
* `"h_description":"{{.MachineName}} ({{.SystemModel}})]"` - The value of "h_description" would be populated with the value of MachineName from the SQL output, followed by the SystemModel, surrounded by brackets;
* `"h_site":"{{.SiteName}}"` - When a string is passed to the h_site field the script attempts to resolve the given site name against the Site entity, and populates this (and h_site_id) with the correct site information. If the site cannot be resolved, the site details are not populated for the Asset record being imported.
* `"h_owned_by":"{{.UserName}}"` - When a valid Hornbill User ID (for a Full or Basic User) is passed to this field, the user is verified on your Hornbill instance, and the tool will complete the h_owned_by and h_owned_by_name columns appropriately.
* `"h_used_by":"{{.UserName}}"` - When a valid Hornbill User ID (for a Full or Basic User) is passed to this field, the user is verified on your Hornbill instance, and the tool will complete the h_used_by and h_used_by_name columns appropriately.
* `"h_company_name":"{{.CompanyName}}"` - When a valid Hornbill Company group name is passed to this field, the company is verified against your Hornbill instance, and the tool will complete the h_company_id and h_company_name columns appropriately.

:::tip
Field names are all **case sensitive**.
:::

Should the column name contain a space (this is more likely when the data is coming from a CSV file, or SQL Server query), the following format will get the data: 

* `{{index . \"Computer name\" }}`

Please be advised that there is still a distinct preference for the column names NOT to contain spaces. 

Although it is preferred that any data normalization is handled in the source data, should some post-production be necessary, this is possible using Go Templates and one or more of the following template transform operations:

* `{{.columnName | Upper}}` - Will UPPERCASE the value contained in columnName
* `{{.columnName | Lower}}` - Will lowercase the value contained in columnName
* `{{.columnName | epoch}}` - Will convert an epoch value to the YYYY-MM-DD HH:II:SS format required for a Hornbill DateTime field
* `{{.columnName | epoch_clear}}` - Will convert an epoch value to the YYYY-MM-DD HH:II:SS format required for a Hornbill DateTime field. Defaulting to CLEAR the column if unable to convert.
* `{{.columnName | date_conversion "date time format of the content in .columnName"}}` - Provide the input format based on the following reference time of Jan 2nd 2006 4 minutes and 5 seconds past 3pm - eg "02/01/2006 15:04:05" will convert the regular UK/European date time format to the format useable in the Hornbill datetime field, whereas "01/02/2006 15:04" will process default US date time. Please note that IF your formatting is already in the Hornbill date time format (2006-01-02 15:04:05), you don't need to convert anything.
* `{{.columnName | date_conversion_clear "date time format of the content in .columnName"}}` - Provide the input format based on the following reference time of Jan 2nd 2006 4 minutes and 5 seconds past 3pm - eg "02/01/2006 15:04:05" will convert the regular UK/European date time format to the format useable in the Hornbill datetime field, whereas "01/02/2006 15:04" will process default US date time. Defaulting to CLEAR the column if unable to convert.

As well as the above transforms, we have also included the [Sprig package for Go templates](https://masterminds.github.io/sprig/), which contains over 70 additional template functions to allow you to perform more advanced transforms on your mapped data.


#### AssetTypeFieldMapping

Maps data in to the type-specific Asset record, so the same rules as AssetGenericFieldMapping, above. For the computer asset class:

`"h_last_logged_on_user":"{{.UserName}}"` - when a valid Hornbill User ID (for a Full or Basic User) is passed to this field, the user is verified on your Hornbill instance, and the tool will complete the `h_last_logged_on_user` column with an appropriate URN value for the user.
