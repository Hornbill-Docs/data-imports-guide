# Configuration Example - Azure Resource Query

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer class assets from Azure Arc, using Azure Resource Query.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Azure Arc asset data. We highly recommend that an Azure administrator review the configuration before using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "azureresourcequery",
    "AzureResourceQuery": {
      "SubscriptionIDs": [],
      "ManagementGroups": [],
      "SoftwareKeysafeKeyID": 0,
      "SoftwareLogAnalyticsWorkspaceID": "your-log-analytics-workspace-id"
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "PreserveShared": false,
      "Clauses": [
        "Resources",
        "where type == \"microsoft.hybridcompute/machines\" and properties.osSku contains  \"Windows Server\""
      ],
      "AssetIdentifier": {
        "SourceColumn": "name",
        "Entity": "Assets",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AssetIDColumn": "{{.vmId}}",
        "AppIDColumn": "{{.Publisher}}{{.SoftwareName}}{{.CurrentVersion}}",
        "Clauses": [
            "ConfigurationData",
            "where ConfigDataType == 'Software'",
            "where _ResourceId == '{{AssetID}}'",
            "project Publisher, SoftwareName, CurrentVersion, SourceComputerId, Computer"
        ],
        "Mapping": {
            "h_app_id":"{{.Publisher}}{{.SoftwareName}}{{.CurrentVersion}}",
            "h_app_name": "{{.SoftwareName}}",
            "h_app_vendor":"{{.Publisher}}",
            "h_app_version":"{{.CurrentVersion}}"
        }
    }
    }
],
"AssetGenericFieldMapping": {
  "h_name": "{{.name}}",
  "h_asset_tag": "{{.name}}",
  "h_description": "From Azure: {{.name}} ({{.properties.identity.principalId}})",
  "h_notes": "{{.properties.provisioningState}} {{.properties.planName}}"
},
"AssetTypeFieldMapping": {
  "h_name": "{{.name}}",
  "h_net_computer_name": "{{.name}}",
  "h_model": "{{.properties.detectedProperties.model}}",
  "h_manufacturer": "{{.properties.detectedProperties.manufacturer}}",
  "h_net_ip_address": "{{ (index (index .properties.networkProfile.networkInterfaces 0).ipAddresses 1).address }}",
  "h_subnet_mask": "{{ (index (index .properties.networkProfile.networkInterfaces 0).ipAddresses 1).subnet.addressPrefix }}",
  "h_net_win_domain": "{{ .properties.domainName }}",
  "h_cpu_info": "{{.properties.detectedProperties.processorNames}}",
  "h_description": "From Azure: {{.name}} ({{.properties.identity.principalId}})",
  "h_memory_info": "{{.properties.detectedProperties.totalPhysicalMemoryInBytes}}",
  "h_os_description": "{{.properties.osSku}}",
  "h_os_type": "{{.properties.osEdition}}",
  "h_os_version": "{{.properties.osVersion}}",
  "h_serial_number": "{{.properties.detectedProperties.serialNumber}}",
  "h_physical_cpus": "{{.properties.detectedProperties.processorCount}}",
  "h_logical_cpus": "{{.properties.detectedProperties.logicalCoreCount}}",
  "h_net_name": "{{.properties.adFqdn}}"
}
```
