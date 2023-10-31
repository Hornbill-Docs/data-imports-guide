# Configuration Example - Cynerio

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from Cynerio.

:::important
The configuration example is provided as-is, and may not be suitable to import your organizations Cynerio asset data. We highly recommend that a Cynerio administrator review the filter, expand returned fields and mappings against your Cynerio account prior to using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "cynerio",
    "Cynerio": {
      "Fields": [
        "id",
        "status",
        "vertical_display_name",
        "severity",
        "department",
        "category",
        "display_name",
        "model",
        "vendor",
        "last_seen",
        "serial_number",
        "first_seen",
        "ip",
        "network.vlan",
        "mac",
        "access_point.name",
        "network.ip_allocation",
        "endpoint_protection.agent",
        "os",
        "firmware_version",
        "hardware_version",
        "software_version",
        "phi",
        "type_display_name"
      ]
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "PreserveShared": false,
      "CynerioFilters": {
        "category": "Server",
        "department": "Radiology"
      },
      "AssetIdentifier": {
        "SourceColumn": "id",
        "Entity": "Asset",
        "EntityColumn": "h_external_id"
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.display_name}}",
    "h_asset_tag": "{{.display_name}}",
    "h_description": "From Cynerio: {{.id}} {{.department}} ({{.vendor}} {{.model}})",
    "h_external_id": "{{.id}}"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.display_name}}",
    "h_net_computer_name": "{{.display_name}}",
    "h_model": "{{.model}}",
    "h_manufacturer": "{{.vendor}}",
    "h_description": "From Cynerio: {{.id}} {{.department}} ({{.vendor}} {{.model}})",
    "h_os_description": "{{.os}}",
    "h_os_version": "{{.software_version}}",
    "h_private_ip_address": "{{.ip}}",
    "h_serial_number": "{{.serialNumber}}"
  }
}
```