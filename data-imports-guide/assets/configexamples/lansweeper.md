# Configuration Example - Lansweeper 7.1

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from a Lansweeper 7.1 database. This means that the Keysafe Key that holds the credentials to authenticate requests to the database should be of type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication). 

In this example, asset records found against all defined asset types will be verified using the AssetName column from the DB query, and the h_name column within the Asset entity in your Hornbill instance.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's LanSweeper asset data. We highly recommend that a DBA review the SQL clauses against your LanSweeper database before using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "mssql",
    "Database": {
      "Authentication": "SQL",
      "Encrypt": false,
      "Query": "SELECT  at.AssetType AS AssetTypeID,  at.AssetTypename AS AssetTypeName,  a.AssetID,  a.AssetUnique,  a.Domain, a.Username AS ADUserID,  a.FQDN,  a.IPAddress,  a.SiteID,  CASE WHEN at.AssetTypename = 'Windows' THEN os.Caption WHEN at.AssetTypename = 'Apple Mac' THEN mos.SystemVersion END AS OperatingSystem,  a.SP,  convert(varchar, a.Firstseen, 20) as FirstSeen,  a.Description,  a.AssetName,  a.Mac,  a.Uptime,  a.Memory,  a.NrProcessors,  a.Processor,  convert(varchar, a.LastChanged, 20) as LastChanged,  os.Caption,  os.ProductType,  convert(varchar, ac.PurchaseDate, 20) as PurchaseDate,  convert(varchar, ac.Warrantydate, 20) as Warrantydate,  ac.Manufacturer,  ac.Model,  ac.Serialnumber,  u.Displayname AS ADUserName  FROM dbo.tblAssets AS a  LEFT JOIN dbo.tsysAssetTypes at ON a.Assettype = at.AssetType  LEFT JOIN dbo.tblOperatingsystem os ON a.AssetID = os.AssetID  LEFT JOIN dbo.tblMacOSInfo mos ON a.AssetID = mos.AssetID LEFT JOIN dbo.tblAssetCustom ac ON a.AssetID = ac.AssetID  LEFT JOIN lansweeperdb.dbo.tblADusers u ON a.Username = u.Username"
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Laptop",
      "OperationType": "Both",
      "Query": "WHERE at.AssetTypename = 'Windows' AND os.ProductType = 1 AND ac.Model = 'Latitude E6320'",
      "AssetIdentifier": {
        "SourceColumn": "AssetName",
        "Entity": "Asset",
        "EntityColumn": "h_name",
        "SourceContractColumn": "",
        "SourceSupplierColumn": ""
      }
    },
    {
      "AssetType": "Desktop",
      "OperationType": "Both",
      "Query": "WHERE (at.AssetTypename = 'Windows' AND os.ProductType = 1 AND ac.Model = 'Precision WorkStation T5500') OR at.AssetTypename = 'Apple Mac'",
      "AssetIdentifier": {
        "SourceColumn": "AssetName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      }
    },
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "Query": "WHERE os.ProductType IN (2, 3)",
      "AssetIdentifier": {
        "SourceColumn": "AssetName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.AssetName}}",
    "h_asset_tag": "{{.AssetName}}",
    "h_created_date": "{{.Firstseen}}",
    "h_description": "{{.Description}}",
    "h_owned_by": "{{.ADUserID}}",
    "h_owned_by_name": "{{.ADUserName}}",
    "h_used_by": "{{.ADUserID}}",
    "h_used_by_name": "{{.ADUserName}}",
    "h_warranty_expires": "{{.Warrantydate}}",
    "h_warranty_start": "{{.PurchaseDate}}"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.AssetName}}",
    "h_mac_address": "{{.Mac}}",
    "h_net_ip_address": "{{.IPAddress}}",
    "h_net_computer_name": "{{.FQDN}}",
    "h_net_win_domain": "{{.Domain}}",
    "h_model": "{{.Model}}",
    "h_manufacturer": "{{.Manufacturer}}",
    "h_cpu_info": "{{.Processor}}",
    "h_description": "{{.Description}}",
    "h_memory_info": "{{.Memory}}",
    "h_os_description": "{{.OperatingSystem}}",
    "h_os_service_pack": "{{.ServicePackVersion}}",
    "h_os_version": "{{.OSCode}}",
    "h_serial_number": "{{.Serialnumber}}",
    "h_physical_cpus": "{{.NrProcessors}}",
    "h_net_name": "{{.FQDN}}"
  }
}
```