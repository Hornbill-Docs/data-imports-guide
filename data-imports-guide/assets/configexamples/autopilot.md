# Configuration Example - Microsoft Autopilot

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from Autopilot.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Autopilot asset data. We highly recommend that an Autopilot administrator review the filter, expand column query and mappings against your Autopilot tenant before using this in a production environment.
:::

```json
{
    "KeysafeKeyID": 0,
    "LogSizeBytes": 1000000,
    "HornbillUserIDColumn": "h_user_id",
    "HornbillNotifyUsers":["user","admin"],
    "SourceConfig": {
        "Source": "autopilot",
        "Autopilot": {
            "Query": ""
        }
    },
    "AssetTypes": [
        {
            "AssetType": "Virtual Machine",
            "OperationType": "Both",
            "PreserveShared": false,
            "AssetIdentifier": {
                "SourceColumn": "serialNumber",
                "Entity": "AssetsComputer",
                "EntityColumn": "h_serial_number"
            },
            "AssetTypeFieldMapping": {
                "h_name": "{{.deviceName}}",
                "h_net_computer_name": "{{.displayName}}",
                "h_model": "{{.model}}",
                "h_manufacturer": "{{.manufacturer}}",
                "h_description": "From Autopilot: {{.displayName}} ({{.model}})\n\n Sku Number: {{.skuNumber}}",
                "h_serial_number": "{{.serialNumber}}",
                "h_cmdb_assets_computer.h_product_id": "{{.productKey}}"
            }
        }
    ],
    "AssetGenericFieldMapping": {
        "h_name": "{{.deviceName}}",
        "h_asset_tag": "{{.deviceName}}",
        "h_description": "From Autopilot: {{.deviceName}} ({{.model}})",
        "h_used_by": "{{.userPrincipalName}}"
    }
}
```
