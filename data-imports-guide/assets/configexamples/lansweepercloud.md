# Configuration Example - Lansweeper Cloud

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import computer-type assets from a Lansweeper Cloud site. This means that the Keysafe Key that holds the credentials to authenticate requests to the Lansweeper Cloud API should be of type [Lansweeper Cloud Authentication](/data-imports-guide/assets/authentication#key-type-lansweeper-cloud). 

In this example, asset records found against all defined asset types will be verified using the .assetBasicInfo.name path from the Lansweeper data, and the h_name column within the Asset entity in your Hornbill instance.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's LanSweeper asset data. We highly recommend that Lansweeper Adnimistrator validates the conditions and Site ID against your LanSweeper Cloud instance before using this in a production environment.
:::

```json
{
    "KeysafeKeyID": 0,
    "LogSizeBytes": 1000000,
    "HornbillUserIDColumn": "h_email",
    "HornbillNotifyUsers":["user","admin"],
    "SourceConfig": {
        "Source": "lansweepercloud",
        "LansweeperCloud": {
            "SiteID": ""
        }
    },
    "AssetTypes": [{
            "AssetType": "Desktop",
            "OperationType": "Both",
            "LansweeperCloudConditions": {
                "Conjunction": "AND",
                "Conditions": [
                    {
                        "Path": "assetBasicInfo.name",
                        "Operator": "EQUAL",
                        "Value": "Workstation1"
                    }
                ]
            },
            "AssetIdentifier": {
                "SourceColumn": "{{.assetBasicInfo.name}}",
                "Entity": "Asset",
                "EntityColumn": "h_name",
                "SourceContractColumn": "",
                "SourceSupplierColumn": ""
            },
            "SoftwareInventory": {
                "AppIDColumn": "{{.publisher}}-{{.name}}-{{.version}}",
                "Mapping": {
                    "h_app_id":"{{.publisher}}-{{.name}}-{{.version}}",
                    "h_app_name": "{{.name}}",
                    "h_app_vendor":"{{.publisher}}",
                    "h_app_version":"{{.version}}",
                    "h_app_install_date":"{{.installDate}}",
                    "h_app_info":"Release: {{.release}}"
                }
            },
            "AssetTypeFieldMapping": {
                "h_name": "{{.assetBasicInfo.name}}",
                "h_mac_address": "{{.networks 0 .macAddress}}",
                "h_net_ip_address": "{{.networks 0 .ipAddressV4}}",
                "h_net_computer_name": "{{.assetCustom.dnsName}}",
                "h_net_win_domain": "{{.assetBasicInfo.domain}}",
                "h_model": "{{.assetCustom.model}}",
                "h_manufacturer": "{{.assetCustom.manufacturer}}",
                "h_description": "Imported from Lansweeper Cloud\n\n{{.assetBasicInfo.name}} - {{.assetCustom.manufacturer}} {{.assetCustom.model}}",
                "h_os_description": "{{.operatingSystem.name}}",
                "h_os_service_pack": "{{.operatingSystem.servicePackMajorVersion}}.{{.operatingSystem.servicePackMinorVersion}}",
                "h_os_version": "{{.operatingSystem.version}}",
                "h_serial_number": "{{.assetCustom.serialNumber}}",
                "h_net_name": "{{.assetCustom.dnsName}}"
            }
        }
    ],
    "AssetGenericFieldMapping": {
        "h_name": "{{.assetBasicInfo.name}}",
        "h_asset_tag": "{{.assetBasicInfo.name}}",
        "h_created_date": "{{.assetBasicInfo.firstSeen}}",
        "h_owned_by": "sometestvalue@email.com",
        "h_description": "Imported from Lansweeper Cloud\n\n{{.assetBasicInfo.name}} - {{.assetCustom.manufacturer}} {{.assetCustom.model}}",
        "h_warranty_expires": "{{.assetCustom.warrantyDate}}",
        "h_warranty_start": "{{.assetCustom.purchaseDate}}"
    }
}
```