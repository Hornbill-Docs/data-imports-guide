# Configuration

## Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

We have provided a selection of default configuration files, one for each of the systems that Hornbill customers most often import contact data from, in each of the downloadable archives. This can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument.
:::

## Example Configuration

```json
{
    "KeysafeKeyID": 0,
    "ContactAction": "Both",
    "AttachCustomerPortal": true,
    "CustomerPortalOrgView": false,
    "CustomerPortalOrgViewRevoke": false,
    "UpdateContactStatus": false,
    "SubscribeToServiceID": 0,
    "SQLConf": {
        "ContactID": "Email Address",
        "FieldID": "h_logon_id"
    },
    "CSVConf": {
        "DataSourceFile":"MasterEmails3.csv"
    },
    "ContactMapping":{
        "logon_id":"{{.FIELD1}}",
        "firstname":"{{.FIELD2}}",
        "lastname":"{{.FIELD3}}",
        "company":"{{.FIELD1}}",
        "email_1":"{{.FIELD2}}",
        "email_2":"",
        "tel_1":"{{.telext}}",
        "tel_2":"",
        "jobtitle":"",
        "description":"",
        "notes":"",
        "country":"",
        "language":"",
        "private":"0",
        "rights":"0",
        "contact_status":"0",
		"custom_1":"",
		"custom_2":"",
		"custom_3":"",
		"custom_4":"",
		"custom_5":"",
		"custom_6":""
    }
}
```

### Configuration Explanation

#### Hornbill Instance Specific Configuration Properties

- `KeySafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your data source authentication details. Set to 0 for importing directly from CSV files. See the [KeySafe section of the Authentication article](/data-imports-guide/assets/authentication#keysafe) for more information on supported key types.
:::tip
The KeySafe Key ID is the unique identifier of the key, and can be found in the URL when you are on the key details form in your browser. In the example below, `4` is the KeySafe Key ID:

`https://live.hornbill.com/yourinstanceid/admin/platform/security/keysafe/4/`

#### SourceConfig

- `SQLConf` - Type: `object` - The SQL configuration for the import
  - `mssql` - Microsoft SQL Server (2005 or above) - will use KeySafe key type [Database Authentication](/data-imports-guide/contacts/authentication#key-type-database-authentication)
  - `mysql` - MySQL Server - will use KeySafe key type [Database Authentication](/data-imports-guide/contacts/authentication#key-type-database-authentication)
  - `mysql320` - MySQL Server v3.2.0 to v4.0 - will use KeySafe key type [Database Authentication](/data-imports-guide/contacts/authentication#key-type-database-authentication)
  - `Encrypt` - Type: `boolean` - Used to specify whether the connection between the script and the database should be encrypted. **NOTE** There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to `true` if your SQL Server has been patched accordingly.
  - `Query` - Type: `string` - The basic SQL query to retrieve contact information from the data source.
- `CSVConf` - Type: `object` - The CSV configuration for the import
  - `DataSourceFile` - CSV / Text file(s) - doesn't require KeySafe keys
   
#### ContactGenericFieldMapping

Maps data in to the generic Contact record Any value templated with `{{.columnName}}` will be populated with the corresponding response from the data source records. Providing a value of `__clear__` will `NULL` that column for the record in the database when contacts are being updated ONLY. This can either be hard-coded in the config, or sent as a string column within the SQL query resultset as so:

```sql
SELECT '__clear__' AS clearColumn
``` 

Then `[clearColumn]` can be used in the mapping, for example.

Any other value is treated as written examples below:

* `"logon_id":"{{.FIELD1}}"` - The value of FIELD1 is taken from the SQL output and populated within this field;
* `"firstname":"{{.FIELD2}}"` - The value of FIELD2 is taken from the SQL output and populated within this field;
* `"lastname":"{{.FIELD3}}"` - The value of FIELD3 is taken from the SQL output and populated within this field;
* `company":"{{.FIELD4}}"` - When a valid Hornbill Company group name is passed to this field, the company is verified against your Hornbill instance, and the tool will complete the h_company_id and h_company_name columns appropriately.
* `"email_1":"{{.FIELD5}}"` - The value of FIELD5 is taken from the SQL output and populated within this field;
* `"email_2":""` - The value is taken from the SQL output and populated within this field;
* `"tel_1":"{{.telext}}"` - The value of telext is taken from the SQL output and populated within this field;
* `"tel_2":""` - The value is taken from the SQL output and populated within this field;
* `"jobtitle":""` - The value is taken from the SQL output and populated within this field;
* `"description":"{{.FIELD1}} ({{.FIELD2}})]"` - The value of "description" would be populated with the values provided to it from the SQL output, surrounded by brackets;
* `"notes":""` - The value of lastname is taken from the SQL output and populated within this field;
* `"country":""` - The value  is taken from the SQL output and populated within this field;
* `"language":""` - The value  is taken from the SQL output and populated within this field;
* `"private":"0"` - The value  is taken from the SQL output and populated within this field;
* `"rights":"0"` - The value  is taken from the SQL output and populated within this field;
* `"contact_status":"0"` - The value  is taken from the SQL output and populated within this field;
* `"custom_1":""` - The value  is taken from the SQL output and populated within this field;
* `"custom_2":""` - The value  is taken from the SQL output and populated within this field;
* `"custom_3":""` - The value  is taken from the SQL output and populated within this field;
* `"custom_4":""` - The value  is taken from the SQL output and populated within this field;
* `"custom_5":""` - The value  is taken from the SQL output and populated within this field;
* `"custom_6":""` - The value  is taken from the SQL output and populated within this field;

:::tip
Field names are all **case sensitive**.
:::

