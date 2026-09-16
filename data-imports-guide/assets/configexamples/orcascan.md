# Configuration Example - Orca Scan

The following is an example of the SourceConfig, AssetTypes and data mapping configuration that could be used to import asset records from an [Orca Scan](https://orcascan.com/) sheet.

The utility reads the sheet using the Orca Scan [live data URL](https://orcascan.com/guides/how-to-get-data-from-orca-scan-in-real-time-3e275a09) feature, requesting the JSON flavor of the feed. The live data URL is read directly by the utility from the computer that it runs on, so that computer must be able to reach `https://api.orcascan.com` over HTTPS.

:::important
The configuration example is provided as-is, and may not be suitable to import your organization's Orca Scan data. The example points at the public Orca Scan demonstration sheet. We highly recommend that an Orca Scan administrator review the sheet ID, returned columns and mappings against your Orca Scan account before using this in a production environment.
:::

:::note
Access to a sheet's live data URL is controlled by the sheet ID in the URL, and the feed does not require a username, password or API key. For this reason the `orcascan` data source does not use a KeySafe key, and `KeysafeKeyID` can be omitted from, or set to `0` in, the configuration file. Treat your sheet ID with the same care as you would a password.
:::

## Enabling the live data URL for a sheet

The live data URL is turned off by default for every Orca Scan sheet. Before the utility can read a sheet, a user with access to that sheet must enable its live data URL in the Orca Scan web app:

1. Log in to the [Orca Scan web app](https://cloud.orcascan.com/) and open the sheet that holds the asset data you want to import.
1. Click `Integrations` in the top menu.
1. Find the `Live Data URL` integration and toggle it on. The sheet's live data URL is then displayed, in the format `https://api.orcascan.com/sheets/[sheet-id]`.
1. Click `Save`.
1. Copy the `[sheet-id]` portion of the URL into the `SheetID` option of your import configuration. Alternatively, copy the whole URL into `EndpointURL` and leave `SheetID` empty.

Repeat these steps for each sheet you want to import from. The full Orca Scan instructions, including the query parameters that the `OrcaScan` configuration options map to, are in the [Orca Scan live data guide](https://orcascan.com/guides/how-to-get-data-from-orca-scan-in-real-time-3e275a09#setup-live-data-url).

:::tip
You can confirm that the URL is working, and see the exact column names to use in your field mappings, by opening `https://api.orcascan.com/sheets/[sheet-id].json` in a browser on the computer that will run the import. Turning the toggle off again in Orca Scan disables the URL and stops the import from reading the sheet.
:::

```json
{
  "LogSizeBytes": 1000000,
  "HornbillUserIDColumn": "h_user_id",
  "HornbillNotifyUsers": ["user","admin"],
  "SourceConfig": {
    "Source": "orcascan",
    "OrcaScan": {
      "EndpointURL": "https://api.orcascan.com/sheets",
      "SheetID": "sJ0KYsnp-9b7Rl7i",
      "Columns": [],
      "SortBy": "",
      "SortOrder": "asc",
      "Limit": 0,
      "History": false,
      "From": "",
      "Deltas": false,
      "ZeroDeltaBase": false,
      "DateTimeFormat": "YYYY-MM-DD HH:mm:ss",
      "Timezone": "",
      "GPS": "city, country",
      "AdditionalParams": {},
      "Headers": {},
      "MaxRetries": 3,
      "ColumnAliases": {
        "Serial Number": "SerialNumber"
      },
      "SanitizeColumnNames": true,
      "TrimValues": true
    }
  },
  "AssetTypes": [
    {
      "AssetType": "Desktop",
      "OperationType": "Both",
      "PreserveShared": false,
      "PreserveState": false,
      "PreserveSubState": false,
      "PreserveOperationalState": false,
      "OrcaScan": {
        "Columns": ["Barcode", "Name", "Quantity", "Description", "Location", "Date"]
      },
      "AdditionalFilters": [
        {
          "Field": "Name",
          "Operator": "NOTEMPTY",
          "Value": ""
        }
      ],
      "AssetIdentifier": {
        "SourceColumn": "Barcode",
        "Entity": "Asset",
        "EntityColumn": "h_asset_tag"
      }
    }
  ],
  "AssetGenericFieldMapping": {
    "h_name": "{{.Name}}",
    "h_asset_tag": "{{.Barcode}}",
    "h_description": "From Orca Scan: {{.Description}}",
    "h_location": "{{.Location}}",
    "h_notes": "Quantity: {{.Quantity}}\nLast scanned: {{.Date}}"
  },
  "AssetTypeFieldMapping": {
    "h_name": "{{.Name}}",
    "h_last_logged_on": "{{.Date}}"
  }
}
```

## How the sheet is read

- Each row in the sheet becomes one source record. The columns of the sheet become the fields that can be used in the `AssetGenericFieldMapping` and `AssetTypeFieldMapping` templates, for example `{{.Barcode}}`.
- Every value in the live data feed is returned by Orca Scan as text, including numbers and dates. Use the data transformation template functions described in the [Configuration](/data-imports-guide/assets/configuration) article if a value needs to be converted before it is written to Hornbill.
- The live data URL returns the whole sheet in a single response. It does not page through results, so use the `Columns` and `Limit` options to keep the response to the data you need.
- Orca Scan can only filter the feed by barcode on the server. To import a subset of rows based on any other column, use the `AdditionalFilters` option against the asset type, as shown in the example above.

## Sheet columns with spaces in their names

Orca Scan sheet columns are often given display names that contain spaces, for example `Serial Number`. A Go template cannot reference such a column directly as `{{.Serial Number}}`. You have three options:

- Set `SanitizeColumnNames` to `true`. Each column is then also made available under a template-friendly name, where any run of characters that are not letters, numbers or underscores is replaced by a single underscore. `Serial Number` becomes `{{.Serial_Number}}`.
- Add an entry to `ColumnAliases`. The example above makes `Serial Number` available as `{{.SerialNumber}}`.
- Use the index form in the template: `{{index . \"Serial Number\"}}`.

The original column name is always kept, so existing mappings are not affected by enabling either option.

## Dates and locations

- `DateTimeFormat` asks Orca Scan to return date and time columns in the given format, using Orca Scan's own tokens (`YYYY`, `MM`, `DD`, `HH`, `mm`, `ss`). The value `YYYY-MM-DD HH:mm:ss` shown in the example returns dates in the exact format that Hornbill date time fields, such as `h_last_logged_on`, expect, so no further conversion is needed in the mapping. If `DateTimeFormat` is not set, dates are returned in ISO 8601 format (`2017-01-01T18:38:17Z`) and must be converted with the `date_conversion` template function before being mapped to a date time field.
- `Timezone` shifts the returned dates by the given UTC offset, for example `+01:00` or `-10:00`.
- `GPS` controls how GPS columns are returned. Orca Scan supports the tokens `lat`, `lng`, `city`, `country` and `code` (country code), combined in any order, for example `city, country` or `lat, lng`. If `GPS` is not set, GPS columns are returned as `latitude, longitude`.

## Revision history

When `History` is set to `true`, Orca Scan returns every recorded change to every row rather than the current values, so the feed can contain many rows for the same barcode. The utility keys source records by the `AssetIdentifier` `SourceColumn`, so where more than one row shares the same identifier the last row returned is the one that is imported. Combine `History` with `SortBy`, `SortOrder` and `From` to control which revision that is. Revision history older than the retention period of your Orca Scan subscription is not available in the feed.

## Configuration options

The `OrcaScan` object can be provided in two places:

- `SourceConfig` > `OrcaScan` - the default options for every asset type in the configuration file.
- `AssetTypes` > `OrcaScan` - options for one asset type. Any option set here overrides the same option from `SourceConfig`, which allows each asset type to read a different sheet, or a different set of columns from the same sheet. Boolean options are enabled if they are set to `true` at either level. `AdditionalParams`, `Headers` and `ColumnAliases` are merged, with the asset type entries taking precedence.

| Option | Type | Description |
| --- | --- | --- |
| `EndpointURL` | `string` | The base Orca Scan sheets endpoint. Defaults to `https://api.orcascan.com/sheets`. May instead be set to the full live data URL of a sheet, with or without the `.json` suffix, in which case `SheetID` should be left empty. The utility always requests the JSON flavor of the feed. |
| `SheetID` | `string` | The ID of the Orca Scan sheet to read, as shown in the sheet's live data URL. Appended to `EndpointURL`. |
| `Columns` | `array` | The sheet columns to return. Only these columns are available to the field mappings. Leave empty to return every column. |
| `Barcode` | `string` | Only return the row (or rows, when `History` is enabled) with this barcode value. |
| `SortBy` | `string` | The column to sort the returned rows by. |
| `SortOrder` | `string` | `asc` or `desc`. |
| `Limit` | `integer` | The maximum number of rows to return. `0` returns every row. |
| `History` | `boolean` | Return all historical changes to the sheet rather than the current values. Defaults to `false`. See [Revision history](#revision-history). |
| `From` | `string` | Only return history from this date onward, in ISO 8601 format, for example `2019-06-12T00:00:00`. Only used when `History` is `true`. |
| `Deltas` | `boolean` | Return the change in numeric columns between revisions rather than the recorded values. Only used when `History` is `true`. |
| `ZeroDeltaBase` | `boolean` | Report the first delta of each numeric column as `0` rather than as its first value. Only used when `Deltas` is `true`. |
| `DateTimeFormat` | `string` | The format that Orca Scan should return date and time columns in, using Orca Scan date tokens. See [Dates and locations](#dates-and-locations). |
| `Timezone` | `string` | UTC offset to apply to date and time columns, for example `+01:00`. |
| `GPS` | `string` | The format that Orca Scan should return GPS columns in, for example `city, country`. |
| `AdditionalParams` | `object` | Any further query string parameters to add to the request, as `"name": "value"` pairs. These are added after the options above and override them where the names match, so new Orca Scan parameters can be used before this documentation is updated. |
| `Headers` | `object` | Additional HTTP request headers to send with the request, as `"name": "value"` pairs. Not needed for the standard live data URL. |
| `MaxRetries` | `integer` | The number of attempts made if Orca Scan responds with `429 Too Many Requests` or a server error. The wait between attempts honors the `Retry-After` response header. Defaults to `3`. |
| `ColumnAliases` | `object` | `"Sheet column name": "alias"` pairs. Each aliased column is also made available to the mapping templates under its alias. See [Sheet columns with spaces in their names](#sheet-columns-with-spaces-in-their-names). |
| `SanitizeColumnNames` | `boolean` | Also make each column available under a template-friendly name. Defaults to `false`. |
| `TrimValues` | `boolean` | Remove leading and trailing white space from every value before it is mapped. Defaults to `false`. |
