# Configuration Example - Nexthink

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from Nexthink.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Nexthink asset data. We highly recommend that a Nexthink administrator review the query and mapping against your Nexthink account before using this in a production environment.
:::

```json
{
  "KeysafeKeyID": 0,
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "SourceConfig": {
    "Source": "nexthink"
  },
  "AssetTypes": [
    {
      "AssetType": "Server",
      "OperationType": "Both",
      "Query": "(select (id name device_manufacturer device_model last_logon_time last_logged_on_user mac_addresses ip_addresses total_ram os_version_and_architecture os_build system_drive_capacity device_serial_number cpu_model cpu_frequency number_of_cores logical_cpu_number bios_serial_number distinguished_name) (from device (where device (eq os_version_and_architecture (pattern 'Windows*Server*')))))",
      "NexthinkPlatform": "windows",
      "AssetIdentifier": {
        "SourceColumn": "name",
        "Entity": "Asset",
        "EntityColumn": "h_name"
      },
      "SoftwareInventory": {
        "AssetIDColumn": "name",
        "Query": "(select ((package (id publisher first_installation name program version)))(from (device package) (with package(where device (eq id (identifier {{AssetID}})))(where package(eq type (enum 'program'))(eq status (enum 'installed')))))(limit 20000))",
        "Mapping": {
          "h_app_id": "{{.publisher}}{{.name}}{{.version}}",
          "h_app_name": "{{.name}}",
          "h_app_vendor": "{{.publisher}}",
          "h_app_version": "{{.version}}",
          "h_app_install_date": "{{.first_installation}}"
        }
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.name}}",
    "h_asset_tag": "{{.name}}",
    "h_description": "From Nexthink: {{.name}} ({{.device_model}})",
    "h_notes": "System Drive Capacity: {{.system_drive_capacity}}\nRAM: {{.total_ram}}",
    "h_used_by": "{{.last_logged_on_user}}"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.name}}",
    "h_mac_address": "{{.mac_addresses}}",
    "h_net_ip_address": "{{.ip_addresses}}",
    "h_net_computer_name": "{{.name}}",
    "h_model": "{{.device_model}}",
    "h_manufacturer": "{{.device_manufacturer}}",
    "h_description": "{{.device_manufacturer .device_model}}",
    "h_last_logged_on": "{{.last_logon_time}}",
    "h_last_logged_on_user": "{{.last_logged_on_user}}",
    "h_memory_info": "{{.total_ram}}",
    "h_os_description": "{{.os_version_and_architecture}}",
    "h_os_version": "{{.os_build}}",
    "h_physical_disk_size": "{{.system_drive_capacity}}",
    "h_serial_number": "{{.device_serial_number}}",
    "h_cpu_info": "{{.cpu_model}}",
    "h_cpu_clock_speed": "{{.cpu_frequency}}",
    "h_physical_cpus": "{{.number_of_cores}}",
    "h_logical_cpus": "{{.logical_cpu_number}}",
    "h_bios_serial_number": "{{.bios_serial_number}}",
    "h_net_name": "{{.distinguished_name}}"
  }
}
```