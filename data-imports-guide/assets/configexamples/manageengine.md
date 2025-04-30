# Configuration Example - Jamf

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import Devices and software assets from Manage Engine.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Manage Engine asset data. We highly recommend that a Manage Engine administrator review the query and mappings against your Manage Engine account before using this in a production environment.
:::

For Devices and Software Assests:
```json
{
    "KeysafeKeyID": 0,
    "LogSizeBytes": 1000000,
    "HornbillUserIDColumn": "h_user_id",
    "SourceConfig": {
        "Source": "manageengine",
        "ManageEngine": {
            "Query": ""
        }
    },
    "AssetTypes": [{
        "AssetType": "Desktop",
        "OperationType": "both",
        "PreserveShared": false,
        "Query": "",
        "AssetIdentifier": {
            "SourceColumn": "{{.resource_id | int}}",
            "Entity": "AssetsSoftware",
            "EntityColumn": "h_name"
        },
        "SoftwareInventory": {
        "AppIDColumn": "{{.software_id | int}}",
        "Query": "",
        "Mapping": {
          "h_app_id": "{{.software_id | int}}",
          "h_app_name": "{{.software_name}}",
          "h_app_vendor": "{{.manufacturer_name}}",
          "h_app_version": "{{.software_version}}",
          "h_app_install_date": "{{.installed_date}}",
          "h_app_help": "",
          "h_app_info": ""
        }
      }
    }],
    "AssetGenericFieldMapping": {
        "h_name": "{{.resource_id | int}}",
        "h_asset_tag": "{{.installed_format}}",
        "h_description": "From Manage Engine: {{.resource_name}} {{(index .software 0).software_name}}"
    },
    "AssetTypeFieldMapping": {
        "h_name": "{{.resource_id | int}}",
        "h_description": "From Manage Engine: ({{.resource_name}}) ({{.manufacturer_name}})"
    }
}
```