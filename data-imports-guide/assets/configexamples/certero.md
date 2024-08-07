# Configuration Example - Certero

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from Certero.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Certero asset data. We highly recommend that a Certero administrator review the filter, expand column query and mappings against your Certero account before using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "certero",
    "Certero": {
      "Expand": "ComputerSystemInventory($select=ComputerSystemObjectId,ComputerDomain,ComputerName,IpAddress,MacAddress,SubnetMask,Manufacturer,Model,Username;$expand=OperatingSystem($select=Caption,VersionString)),ComputerSystemProcessorInfo($select=ClockSpeed,Cores,CoresPerCpu,Manufacturer,Model),WindowsSystemSoftwareProducts($select=SoftwareProductObjectId,WindowsSystemObjectId,InstallDate,SupportUrl,UninstallString,SoftwareProduct;$expand=SoftwareProduct($select=SoftwareProductObjectId,DisplayName,ProductCode,Publisher,Version))"
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "Query": "startswith(ComputerSystemInventory/OperatingSystem/Caption,'Windows Server')",
      "AssetIdentifier": {
        "SourceColumn": "{{.ComputerSystemInventory.ComputerName}}",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AppIDColumn": "{{.SoftwareProduct.ProductCode}}",
        "ParentObject": "WindowsSystemSoftwareProducts",
        "Mapping": {
          "h_app_id": "{{.SoftwareProduct.ProductCode}}",
          "h_app_name": "{{.SoftwareProduct.DisplayName}}",
          "h_app_vendor": "{{.SoftwareProduct.Publisher}}",
          "h_app_version": "{{.SoftwareProduct.Version}}",
          "h_app_install_date": "{{.InstallDate}}",
          "h_app_help": "{{.SupportUrl}}"
        }
      }
    },
    {
      "AssetType": "Desktop",
      "OperationType": "Both",
      "Query": "contains(ComputerSystemInventory/OperatingSystem/Caption,'Windows') and contains(ComputerSystemInventory/OperatingSystem/Caption,'Server') eq false",
      "AssetIdentifier": {
        "SourceColumn": "{{.ComputerSystemInventory.ComputerName}}",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AppIDColumn": "{{.SoftwareProduct.ProductCode}}",
        "ParentObject": "WindowsSystemSoftwareProducts",
        "Mapping": {
          "h_app_id": "{{.SoftwareProduct.ProductCode}}",
          "h_app_name": "{{.SoftwareProduct.DisplayName}}",
          "h_app_vendor": "{{.SoftwareProduct.Publisher}}",
          "h_app_version": "{{.SoftwareProduct.Version}}",
          "h_app_install_date": "{{.InstallDate}}",
          "h_app_help": "{{.SupportUrl}}"
        }
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.ComputerSystemInventory.ComputerName}}",
    "h_asset_tag": "{{.ComputerSystemInventory.ComputerName}}",
    "h_description": "From Certero: {{.ComputerSystemInventory.Manufacturer}} ({{.ComputerSystemInventory.Model}})",
    "h_external_id": "{{.ComputerSystemInventory.ComputerSystemObjectId}}",
    "h_external_source": "Certero"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.ComputerSystemInventory.ComputerName}}",
    "h_mac_address": "{{.ComputerSystemInventory.MacAddress}}",
    "h_net_ip_address": "{{.ComputerSystemInventory.IpAddress}}",
    "h_net_computer_name": "{{.ComputerSystemInventory.ComputerName}}",
    "h_model": "{{.ComputerSystemInventory.Model}}",
    "h_description": "From Certero: {{.ComputerSystemInventory.Manufacturer}} ({{.ComputerSystemInventory.Model}})",
    "h_os_description": "{{.ComputerSystemInventory.OperatingSystem.Caption}}",
    "h_os_version": "{{.ComputerSystemInventory.OperatingSystem.VersionString}}",
    "h_cpu_info": "{{.ComputerSystemProcessorInfo.Model}}",
    "h_cpu_clock_speed": "{{.ComputerSystemProcessorInfo.ClockSpeed}}",
    "h_physical_cores": "{{.ComputerSystemProcessorInfo.Cores}}",
    "h_last_logged_on_user": "{{.ComputerSystemInventory.Username}}",
    "h_subnet_mask": "{{.ComputerSystemInventory.SubnetMask}}"
  }
}
```