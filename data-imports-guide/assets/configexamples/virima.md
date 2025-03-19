# Configuration Example - Virima

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import Windows Server CIs from Virima.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Virima CI data. We highly recommend that a Virima administrator review the query and mapping against your Virima account before using this in a production environment.
:::

```json
{
    "KeysafeKeyID": 0,
    "LogSizeBytes": 1000000,
    "HornbillUserIDColumn": "h_user_id",
    "SourceConfig": {
        "Source": "virima",
        "Virima": {
            "Module": "CmdbCi",
            "ClassName": "CmdbCi",
            "DBClassName": "TCI",
            "Sort": {
                "SortKey": "lastModifiedOn",
                "SortOrder": "DESC"
            }
        }
    },
    "AssetTypes": [{
        "AssetType": "Windows Servers",
        "OperationType": "Both",
        "VirimaFilters": [
            {
                "dbPropertyType": "boolean",
                "filterKey": "archive",
                "operator": "eq",
                "value1": "false"
            },
            {      
                "dbPropertyType": "boolean",
                "filterKey": "blueprint.isCIPart",
                "operator": "eq",
                "value1": "false"
            },
            {
                "filterKey": "blueprint.name",
                "operator": "cn",
                "dbPropertyType": "String",
                "value1": "windows server"
              }
        ],
        "PreserveShared": false,
        "PreserveState": false,
        "PreserveSubState": false,
        "PreserveOperationalState": false,
        "AssetIdentifier": {
            "SourceColumn": "{{.id}}",
            "Entity": "Asset",
            "EntityColumn": "h_external_id"
        },
        "SoftwareInventory": {
            "VirimaFilters": [
                {
                    "dbPropertyType": "boolean",
                    "default": "true",
                    "filterKey": "source.archive",
                    "operator": "eq",
                    "value1": "false"
                }
            ],
            "AppIDColumn": "{{.Software_Publisher}}{{.Software_Name}}{{.Software_Version}}",
            "Mapping": {
                "h_app_id":"{{.Software_Publisher}}{{.Software_Name}}{{.Software_Version}}",
                "h_app_name": "{{.Software_Name}}",
                "h_app_vendor":"{{.Software_Publisher}}",
                "h_app_version":"{{.Software_Version}}",
                "h_app_info":"",
                "h_app_install_date": "{{ if .Software_Install_Date }} {{ toDate \"20060102\" .Software_Install_Date | date \"2006-01-02 15:05:05\" }} {{ end }}"
            }
        },
        "AssetTypeFieldMapping": {
            "h_name": "{{.Asset_Name}}",
            "h_net_ip_address": "{{.IP_Address}}",
            "h_net_computer_name": "{{.Host_Name}}",
            "h_description": "From Virima: {{.Description}}",
            "h_os_description": "{{.Operating_System}}"
        }
    }],
    "AssetGenericFieldMapping": {
        "h_name": "{{.Asset_Name}}",
        "h_asset_tag": "{{.Asset_Id}}",
        "h_description": "From Virima {{.Description}}",
        "h_external_id": "{{.id}}",
        "h_external_source": "Virima"
    }
}
```