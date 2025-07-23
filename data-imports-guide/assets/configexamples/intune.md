# Configuration Example - Microsoft Intune

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from Intune.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Intune asset data. We highly recommend that an Intune administrator review the filter, expand column query and mappings against your Intune tenant before using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "intune",
    "Intune": {
        "Fields": [
            "id",
            "userId",
            "deviceName",
            "managedDeviceOwnerType",
            "enrolledDateTime",
            "lastSyncDateTime",
            "operatingSystem",
            "osVersion",
            "emailAddress",
            "azureADDeviceId",
            "userPrincipalName",
            "model",
            "manufacturer",
            "serialNumber",
            "userDisplayName",
            "totalStorageSpaceInBytes",
            "managedDeviceName",
            "physicalMemoryInBytes",
            "imei"
        ],
        "ImportDetectedApps": true
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Virtual Machine",
      "OperationType": "Both",
      "Query": "operatingSystem eq 'Windows' and model eq 'Virtual Machine'",
      "AssetIdentifier": {
        "SourceColumn": "serialNumber",
        "Entity": "AssetsComputer",
        "EntityColumn": "h_serial_number"
      },
      "SoftwareInventory": {
        "Mapping": {
          "h_app_id":"{{.id}}",
          "h_app_name": "{{.displayName}}",
          "h_app_vendor":"{{.publisher}}",
          "h_app_version":"{{.version}}"
        }
      },
      "AssetTypeFieldMapping": {
        "h_name": "{{.deviceName}}",
        "h_net_computer_name": "{{.deviceName}}",
        "h_model": "{{.model}}",
        "h_manufacturer": "{{.manufacturer}}",
        "h_description": "From Intune: {{.deviceName}} ({{.model}})",
        "h_memory_info": "{{.physicalMemoryInBytes}}",
        "h_os_description": "{{.operatingSystem}}",
        "h_os_version": "{{.osVersion}}",
        "h_serial_number": "{{.serialNumber}}"
      }
    },
    {
      "AssetType": "iPhone",
      "OperationType": "Both",
      "Query": "startsWith(model, 'iPhone')",
      "AssetIdentifier": {
        "SourceColumn": "serialNumber",
        "Entity": "AssetsMobileDevice",
        "EntityColumn": "h_serial_number"
      },
      "SoftwareInventory": {
        "Mapping": {
          "h_app_id":"{{.id}}",
          "h_app_name": "{{.displayName}}",
          "h_app_vendor":"{{.publisher}}",
          "h_app_version":"{{.version}}"
        }
      },
      "AssetTypeFieldMapping": {
        "h_name": "{{.deviceName}}",
        "h_imei_number": "{{.imei}}",
        "h_model": "{{.model}}",
        "h_manufacturer": "{{.manufacturer}}",
        "h_description": "From Intune: {{.deviceName}} ({{.model}})",
        "h_memory_info": "{{.physicalMemoryInBytes}}",
        "h_os_description": "{{.operatingSystem}}",
        "h_os_version": "{{.osVersion}}",
        "h_serial_number": "{{.serialNumber}}"
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.deviceName}}",
    "h_asset_tag": "{{.deviceName}}",
    "h_description": "From Intune: {{.deviceName}} ({{.model}})",
    "h_used_by": "{{ .userPrincipalName }}"
  }
}
```