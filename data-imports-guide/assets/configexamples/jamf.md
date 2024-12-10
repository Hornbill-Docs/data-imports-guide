# Configuration Example - Jamf

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type or mobile device assets from Jamf.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Jamf asset data. We highly recommend that a Jamf administrator review the query and mappings against your Jamf account before using this in a production environment.
:::

For Computer Assests:
```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
    "SourceConfig": {
        "Source": "jamf",
        "Jamf": {
            "inventoryType": "Computer",
            "Query": ""
        }
    },
      "AssetTypes": [{
        "AssetType": "Laptop",
        "OperationType": "both",
        "PreserveShared": false,
        "Query": "",
        "AssetIdentifier": {
            "SourceColumn": "id",
            "Entity": "Asset",
            "EntityColumn": "h_name"
        }
    }],
    "AssetGenericFieldMapping": {
        "h_name": "{{.id}}",
        "h_asset_tag": "{{.general.assetTag}}",
        "h_description": "From Jamf: {{.general.name}} ({{.general.platform}})"
    },
    "AssetTypeFieldMapping": {
        "h_name": "{{.id}}",
        "h_mac_address": "{{.general.macAddress}}",
        "h_net_ip_address": "{{.general.lastIpAddress}}",
        "h_net_computer_name": "{{.general.name}}",
        "h_description": "From Jamf: {{.general.name}} ({{.hardware.model}})",
        "h_memory_info": "{{.storage}}",
        "h_os_description": "{{.hardware.batteryHealth}}",
        "h_serial_number": "{{.hardware.serialNumber}}",
        "h_os_version": "{{.operatingSystem}}"
    }
}
```

For Mobile Assets:
```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
    "SourceConfig": {
        "Source": "jamf",
        "Jamf": {
            "inventoryType": "Mobile",
            "Query": ""
        }
    },
      "AssetTypes": [{
        "AssetType": "Smart Phone",
        "OperationType": "both",
        "PreserveShared": false,
        "Query": "",
        "AssetIdentifier": {
            "SourceColumn": "mobileDeviceId",
            "Entity": "Asset",
            "EntityColumn": "h_name"
        }
    }],
    "AssetGenericFieldMapping": {
        "h_name": "{{.mobileDeviceId}}",
        "h_asset_tag": "{{.hardware.assetTag}}",
        "h_description": "From Jamf: {{.userAndLocation.username}} ({{.hardware.model}})"
    },
    "AssetTypeFieldMapping": {
        "h_name": "{{.mobileDeviceId}}",
        "h_mac_address": "{{.hardware.bluetoothMacAddress}}",
        "h_net_ip_address": "{{.hardware.wifiMacAddress}}",
        "h_net_computer_name": "{{.userAndLocation.name}}",
        "h_description": "From Jamf: {{.userAndLocation.username}} ({{.hardware.model}})",
        "h_memory_info": "{{.hardware.usedSpacePercentage}}",
        "h_serial_number": "{{.hardware.serialNumber}}",
        "h_model": "{{.hardware.model}}",
        "h_memory_info": "{{.hardware.availableSpaceMb}}",
        "h_physical_disk_size": "{{.hardware.capacityMb}}"
    }
}
```