# Configuration Example - SCCM 2007 & 2012

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from an SCCM 2007 or 2012  database. This means that the Keysafe Key that holds the credentials to authenticate requests to the database should be of type [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication). 

In this example:

* Asset records found against the Server asset type will be verified using the SystemSerialNumber column from the DB query, and the h_serial_number column within the AssetsComputer entity in your Hornbill instance.
* Asset records found against all other defined asset types will be verified using the MachineName column from the DB query, and the h_namecolumn within the Asset entity in your Hornbill instance.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's SCCM asset data. We highly recommend that a DBA review the SQL clauses against your SCCM database before using this in a production environment.
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
      "Query": "SELECT dbo.v_R_System.ResourceID AS [AssetID], dbo.v_R_System.User_Name0 AS [UserName], dbo.v_R_System.Netbios_Name0 AS [MachineName], dbo.v_R_System.Resource_Domain_OR_Workgr0 AS [NETDomain], dbo.v_GS_OPERATING_SYSTEM.Caption0 AS [OperatingSystemCaption], dbo.v_R_System.Operating_System_Name_and0 AS [OperatingSystem], dbo.v_GS_OPERATING_SYSTEM.Version0 AS [OperatingSystemVersion], dbo.v_GS_OPERATING_SYSTEM.CSDVersion0 AS [ServicePackVersion], dbo.v_GS_COMPUTER_SYSTEM.Manufacturer0 AS [SystemManufacturer], dbo.v_GS_COMPUTER_SYSTEM.Model0 AS [SystemModel], dbo.v_GS_PC_BIOS.SerialNumber0 AS [SystemSerialNumber], OAProc.MaxClockSpeed0 AS [ProcessorSpeedGHz], OAProc.Name0 AS [ProcessorName], dbo.v_GS_COMPUTER_SYSTEM.NumberOfProcessors0 AS [NumberofProcessors], dbo.v_GS_X86_PC_MEMORY.TotalPhysicalMemory0 AS [MemoryKB], dbo.v_GS_LOGICAL_DISK.Size0 AS [DiskSpaceMB], dbo.v_GS_LOGICAL_DISK.FreeSpace0 AS [FreeDiskSpaceMB], OAIP.IP_Addresses0 AS [IPAddress], OAMac.MAC_Addresses0 AS [MACAddress], dbo.v_GS_PC_BIOS.Description0 AS [BIOSDescription], dbo.v_GS_PC_BIOS.ReleaseDate0 AS [BIOSReleaseDate], dbo.v_GS_PC_BIOS.SMBIOSBIOSVersion0 AS [SMBIOSVersion], dbo.v_GS_SYSTEM.SystemRole0 AS [SystemType], OASysEncl.ChassisTypes0 AS [ChassisTypes], OASysEncl.TimeStamp AS [ChassisDate], dbo.v_R_System.AD_Site_Name0 AS [SiteName] FROM dbo.v_R_System OUTER APPLY (SELECT TOP 1 dbo.v_GS_SYSTEM_ENCLOSURE.* FROM dbo.v_GS_SYSTEM_ENCLOSURE WHERE dbo.v_GS_SYSTEM_ENCLOSURE.ResourceID = dbo.v_R_System.ResourceID ORDER BY TimeStamp DESC) OASysEncl OUTER APPLY (SELECT TOP 1 IP_Addresses0, ROW_NUMBER() OVER (order by (SELECT 0)) AS rowNum FROM dbo.v_RA_System_IPAddresses WHERE dbo.v_RA_System_IPAddresses.ResourceID = dbo.v_R_System.ResourceID ORDER BY rowNum DESC) OAIP OUTER APPLY (SELECT TOP 1 MAC_Addresses0 FROM dbo.v_RA_System_MACAddresses WHERE dbo.v_RA_System_MACAddresses.ResourceID = dbo.v_R_System.ResourceID ) OAMac OUTER APPLY (SELECT TOP 1 MaxClockSpeed0, Name0 FROM dbo.v_GS_PROCESSOR WHERE dbo.v_GS_PROCESSOR.ResourceID = dbo.v_R_System.ResourceID ORDER BY TimeStamp DESC) OAProc LEFT JOIN dbo.v_GS_X86_PC_MEMORY ON dbo.v_GS_X86_PC_MEMORY.ResourceID = dbo.v_R_System.ResourceID LEFT JOIN dbo.v_GS_OPERATING_SYSTEM ON dbo.v_GS_OPERATING_SYSTEM.ResourceID = dbo.v_R_System.ResourceID LEFT JOIN dbo.v_GS_COMPUTER_SYSTEM ON dbo.v_GS_COMPUTER_SYSTEM.ResourceID = dbo.v_R_System.ResourceID LEFT JOIN dbo.v_GS_PC_BIOS ON dbo.v_GS_PC_BIOS.ResourceID = dbo.v_R_System.ResourceID LEFT JOIN dbo.v_GS_LOGICAL_DISK ON dbo.v_GS_LOGICAL_DISK.ResourceID = dbo.v_R_System.ResourceID LEFT JOIN dbo.v_FullCollectionMembership ON (dbo.v_FullCollectionMembership.ResourceID = v_R_System.ResourceID) LEFT JOIN dbo.v_GS_SYSTEM ON dbo.v_GS_SYSTEM.ResourceID = dbo.v_R_System.ResourceID WHERE dbo.v_GS_LOGICAL_DISK.DeviceID0 = 'C:' AND dbo.v_FullCollectionMembership.CollectionID = 'SMS00001' "
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Desktop",
      "OperationType": "Both",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "Query": "AND OASysEncl.ChassisTypes0 IN (3, 4, 5, 6, 7, 12, 13, 15, 16) AND dbo.v_R_System.Obsolete0 = 0 ORDER BY dbo.v_R_System.ResourceID ASC",
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name",
        "SourceContractColumn": "",
        "SourceSupplierColumn": ""
      },
      "SoftwareInventory": {
        "AssetIDColumn": "AssetID",
        "AppIDColumn": "AppID",
        "Query": "SELECT AppID = CASE WHEN Publisher0 IS NULL AND Version0 IS NULL THEN DisplayName0 WHEN Publisher0 IS NOT NULL AND Version0 IS NULL THEN Publisher0+DisplayName0 ELSE Publisher0+DisplayName0+Version0 END, DisplayName0 , Version0, FCM.Name, convert(datetime, InstallDate0, 112) AS InstallDate0, Publisher0, ProdID0, FCM.ResourceID FROM v_Add_Remove_Programs AS ARP JOIN v_FullCollectionMembership As FCM on ARP.ResourceID=FCM.ResourceID WHERE FCM.CollectionID = 'SMS00001' AND FCM.ResourceID = '{{AssetID}}' AND DisplayName0 IS NOT NULL AND DisplayName0 != '' AND DisplayName0 NOT LIKE '%Update for Windows%' ORDER BY ProdID0 ASC",
        "Mapping": {
          "h_app_id": "{{.AppID}}",
          "h_app_name": "{{.DisplayName0}}",
          "h_app_vendor": "{{.Publisher0}}",
          "h_app_version": "{{.Version0}}",
          "h_app_install_date": "{{.InstallDate0}}",
          "h_app_help": "",
          "h_app_info": ""
        }
      }
    },
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "Query": "AND OASysEncl.ChassisTypes0 IN (2, 17, 18, 19, 20, 21, 22, 23) AND dbo.v_R_System.Obsolete0 = 0 ORDER BY dbo.v_R_System.ResourceID ASC",
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AssetIDColumn": "AssetID",
        "AppIDColumn": "AppID",
        "Query": "SELECT AppID = CASE WHEN Publisher0 IS NULL AND Version0 IS NULL THEN DisplayName0 WHEN Publisher0 IS NOT NULL AND Version0 IS NULL THEN Publisher0+DisplayName0 ELSE Publisher0+DisplayName0+Version0 END, DisplayName0 , Version0, FCM.Name, convert(datetime, InstallDate0, 112) AS InstallDate0, Publisher0, ProdID0, FCM.ResourceID FROM v_Add_Remove_Programs AS ARP JOIN v_FullCollectionMembership As FCM on ARP.ResourceID=FCM.ResourceID WHERE FCM.CollectionID = 'SMS00001' AND FCM.ResourceID = '{{AssetID}}' AND DisplayName0 IS NOT NULL AND DisplayName0 != '' AND DisplayName0 NOT LIKE '%Update for Windows%' ORDER BY ProdID0 ASC",
        "Mapping": {
          "h_app_id": "{{.AppID}}",
          "h_app_name": "{{.DisplayName0}}",
          "h_app_vendor": "{{.Publisher0}}",
          "h_app_version": "{{.Version0}}",
          "h_app_install_date": "{{.InstallDate0}}",
          "h_app_help": "",
          "h_app_info": ""
        }
      }
    },
    {
      "AssetType": "Virtual Machine",
      "OperationType": "Both",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "Query": "AND OASysEncl.ChassisTypes0 = 1 AND dbo.v_R_System.Obsolete0 = 0 ORDER BY dbo.v_R_System.ResourceID ASC",
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AssetIDColumn": "AssetID",
        "AppIDColumn": "AppID",
        "Query": "SELECT AppID = CASE WHEN Publisher0 IS NULL AND Version0 IS NULL THEN DisplayName0 WHEN Publisher0 IS NOT NULL AND Version0 IS NULL THEN Publisher0+DisplayName0 ELSE Publisher0+DisplayName0+Version0 END, DisplayName0 , Version0, FCM.Name, convert(datetime, InstallDate0, 112) AS InstallDate0, Publisher0, ProdID0, FCM.ResourceID FROM v_Add_Remove_Programs AS ARP JOIN v_FullCollectionMembership As FCM on ARP.ResourceID=FCM.ResourceID WHERE FCM.CollectionID = 'SMS00001' AND FCM.ResourceID = '{{AssetID}}' AND DisplayName0 IS NOT NULL AND DisplayName0 != '' AND DisplayName0 NOT LIKE '%Update for Windows%' ORDER BY ProdID0 ASC",
        "Mapping": {
          "h_app_id": "{{.AppID}}",
          "h_app_name": "{{.DisplayName0}}",
          "h_app_vendor": "{{.Publisher0}}",
          "h_app_version": "{{.Version0}}",
          "h_app_install_date": "{{.InstallDate0}}",
          "h_app_help": "",
          "h_app_info": ""
        }
      }
    },
    {
      "AssetType": "Laptop",
      "OperationType": "Both",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "Query": "AND OASysEncl.ChassisTypes0 IN (8, 9, 10, 14) AND dbo.v_R_System.Obsolete0 = 0 ORDER BY dbo.v_R_System.ResourceID ASC",
      "AssetIdentifier": {
        "SourceColumn": "MachineName",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AssetIDColumn": "AssetID",
        "AppIDColumn": "AppID",
        "Query": "SELECT AppID = CASE WHEN Publisher0 IS NULL AND Version0 IS NULL THEN DisplayName0 WHEN Publisher0 IS NOT NULL AND Version0 IS NULL THEN Publisher0+DisplayName0 ELSE Publisher0+DisplayName0+Version0 END, DisplayName0 , Version0, FCM.Name, convert(datetime, InstallDate0, 112) AS InstallDate0, Publisher0, ProdID0, FCM.ResourceID FROM v_Add_Remove_Programs AS ARP JOIN v_FullCollectionMembership As FCM on ARP.ResourceID=FCM.ResourceID WHERE FCM.CollectionID = 'SMS00001' AND FCM.ResourceID = '{{AssetID}}' AND DisplayName0 IS NOT NULL AND DisplayName0 != '' AND DisplayName0 NOT LIKE '%Update for Windows%' ORDER BY ProdID0 ASC",
        "Mapping": {
          "h_app_id": "{{.AppID}}",
          "h_app_name": "{{.DisplayName0}}",
          "h_app_vendor": "{{.Publisher0}}",
          "h_app_version": "{{.Version0}}",
          "h_app_install_date": "{{.InstallDate0}}",
          "h_app_help": "",
          "h_app_info": ""
        }
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.MachineName}}",
    "h_site": "{{.SiteName}}",
    "h_asset_tag": "{{.MachineName}}",
    "h_description": "{{.MachineName}} ({{.SystemModel}})",
    "h_owned_by": "{{.UserName}}",
    "h_used_by": "{{.UserName}}"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.MachineName}}",
    "h_mac_address": "{{.MACAddress}}",
    "h_net_ip_address": "{{.IPAddress}}",
    "h_net_computer_name": "{{.MachineName}}",
    "h_net_win_domain": "{{.NETDomain}}",
    "h_model": "{{.SystemModel}}",
    "h_manufacturer": "{{.SystemManufacturer}}",
    "h_cpu_info": "{{.ProcessorName}}",
    "h_description": "{{.SystemModel}}",
    "h_memory_info": "{{.MemoryKB}}",
    "h_os_description": "{{.OperatingSystem}}",
    "h_os_service_pack": "{{.ServicePackVersion}}",
    "h_os_version": "{{.OperatingSystemVersion}}",
    "h_physical_disk_size": "{{.DiskSpaceMB}}",
    "h_serial_number": "{{.SystemSerialNumber}}",
    "h_cpu_clock_speed": "{{.ProcessorSpeedGHz}}",
    "h_physical_cpus": "{{.NumberofProcessors}}",
    "h_bios_name": "{{.BIOSDescription}}",
    "h_bios_release_date": "{{.BIOSReleaseDate}}",
    "h_bios_version": "{{.SMBIOSVersion}}"
  }
}
```