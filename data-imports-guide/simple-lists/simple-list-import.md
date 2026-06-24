# Simple List Import Utility

The utility provides a simple, safe and secure way to populate list data from a database or ODBC connection into any of the Simple Lists within any of the applications within the Hornbill platform. The tool is designed to run behind your corporate firewall, and requires access to your data host(s) or ODBC connection.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access you should be able to use the tool without any firewall configuration changes.

When the tool is executed it will:

- Extract list data as per your specification (outlined in the Configuration section)
- Create new list entries within the configured application's Simple List
- The list item value must be unique within the application's particular list; duplicates are skipped

## Download

The utility can be downloaded from [GitHub](https://github.com/hornbill/simple-list-import/releases/latest). Please ensure you download the latest version of the ZIP archive relevant to the architecture of the computer it will run on.

## Installation Overview

### Windows Installation

1. Download the OS and architecture-specific ZIP archive
2. Extract the zip into a folder, e.g. `C:\list_import\`
3. Open `conf.json` and add the necessary configuration
4. Open a Command Prompt as Administrator
5. Change directory to the folder containing the utility: `C:\list_import\`
6. Run a dry run to validate your configuration before importing:

```
goHornbillSimpleListImport.exe -dryrun=true
```

## Configuration Overview

A demonstration configuration file (`conf.json`) is provided within the package. If a configuration file is not specified as a command line argument when executing the tool, a default file named `conf.json` must exist in the same directory.

### Example Configuration File

```json
{
  "HBConf": {
    "APIKeysKeysafeID": 1
  },
  "AppDBConf": {
    "Driver": "mysql",
    "KeysafeKeyID": 2,
    "Encrypt": false
  },
  "Lists": [
    {
      "SQL": "SELECT val, disp FROM swlists",
      "Rebuild": false,
      "Application": "com.hornbill.servicemanager",
      "ListName": "someList",
      "ItemValue": "[val]",
      "DefaultDisplay": "[disp]",
      "Translations": [
        {
          "Language": "en-GB",
          "Display": "[disp]"
        }
      ]
    }
  ]
}
```

## What Do I Put in the Configuration File?

### HBConf

Connection information for the Hornbill instance:

- **`APIKeysKeysafeID`** — The integer ID of a Keysafe entry of type **API Key**. This key is used to authenticate all API calls made to your Hornbill instance into the Simple Lists. The Keysafe entry must contain a valid API key created against a user account with sufficient rights.

> Your Instance ID and API key are stored securely in an encrypted `import.cfg` file created on first run — they are not stored in `conf.json`. See [On First Run](#on-first-run).

### AppDBConf

Connection information for the database (direct or via ODBC) that contains the data to import:

- **`Driver`** — The driver to use when connecting to the database:
  - `swsql` — Supportworks 7.x SQL (MySQL v4.0.16). Also supports MySQL v3.2.0 to &lt;v5.0
  - `mysql` — MySQL Server v5.0 or above, or MariaDB
  - `mssql` — Microsoft SQL Server (2005 or above)
  - `odbc` — An ODBC connection on the machine performing the import
  - `csv` — An ODBC connection using the Microsoft Access Text Driver, configured to point at a folder containing CSV files
- **`KeysafeKeyID`** — The integer ID of a Keysafe entry of type **Database Authentication**. This entry holds the server address, database name, username, password, and port used to connect to your data source.
- **`Encrypt`** — Boolean. Whether the connection between the tool and the database should be encrypted. **Note:** There is a known bug in SQL Server 2008 and below that causes the connection to fail when encryption is enabled. Only set to `true` if your SQL Server has been patched accordingly.

#### Database Authentication Keysafe Entry

The Keysafe entry referenced by `AppDBConf.KeysafeKeyID` must be of type **Database Authentication** and contain the following fields:

| Field | Description |
|-------|-------------|
| Server | The address of the database host. Use `127.0.0.1` for ODBC or CSV drivers |
| Username | The SQL database username |
| Password | The password for the above username |
| Database | The database name (or DSN name for ODBC/CSV connections) |
| Port | The SQL port (if connecting directly to a database) |

### Lists

A JSON array of objects, each defining a Simple List to populate:

- **`SQL`** — The SQL query to retrieve data from the configured data source
- **`Rebuild`** — Boolean. If `true`, the specified list will be **deleted in its entirety** before new items are inserted
- **`Application`** — The technical application name for which the Simple List items are to be added (e.g. `com.hornbill.servicemanager` for Service Manager)
- **`ListName`** — The name of the Simple List within the application. The list will be created if it does not already exist
- **`ItemValue`** — The value for each list item. Must be unique within the list; items with duplicate values will be skipped. Use square brackets to reference a column from the SQL result, e.g. `[val]`
- **`DefaultDisplay`** — The display name for each list item. Use square brackets to reference a column from the SQL result, e.g. `[disp]`
- **`Translations`** — A JSON array of translation objects for the display name:
  - **`Language`** — The language code (e.g. `en-GB`). Can be sourced from a SQL column using square brackets
  - **`Display`** — The translated display value. Use square brackets to reference a SQL column, e.g. `[disp]`

## Command Line Parameters

| Flag | Default | Description |
|------|---------|-------------|
| `-file` | `conf.json` | Name of the configuration file to load |
| `-dryrun` | `false` | When `true`, no list items are created. The XML that would be sent to Hornbill is written to the log file instead — use this to validate mappings before a live run |
| `-version` | — | Outputs the current version number and exits |
| `-creds` | — | Prompts for your Instance ID and, if it matches the stored value, displays the stored API key |

## API Key Requirements

The API key (sourced via the Keysafe entry referenced by `HBConf.APIKeysKeysafeID`) requires the following API permissions:

- `data:listAddItem`
- `data:listDelete`
- `admin:keysafeGetKey`

## On First Run

When no `import.cfg` file exists in the tool's directory, you will be prompted for two pieces of information:

1. **Instance ID** — The name of your Hornbill instance, found in the URL: `https://live.hornbill.com/`**instanceid**`/` (case sensitive)
2. **API Key** — A valid 32-character API key created against a Hornbill user account with sufficient rights (case sensitive). Details on creating an API key can be found in the Hornbill documentation. This key is used to connect to your instance and access keysafe.

These details are encrypted and saved to `import.cfg`. The file is tied to the local user account and computer — only the user who created it can run the tool until the file is deleted.

To verify stored credentials at any time, run the tool with the `-creds` flag.

## Preparing to Run

1. Ensure the required Keysafe entries exist in your Hornbill instance (API Key and Database Authentication)
2. Open `conf.json` and add the necessary configuration
3. Open a Command Prompt as Administrator
4. Change directory to the folder containing the utility
5. Run a dry run first to validate your data mappings:

```
goHornbillSimpleListImport.exe -dryrun=true -file=conf.json
```

6. Review the log file to confirm the XML output looks correct, then run without `-dryrun` to perform the import

## HTTP Proxies

If you route internet traffic through a proxy, set the following environment variables before running the tool:

```
set HTTP_PROXY=HOST:PORT
set HTTPS_PROXY=HOST:PORT
```

Where `HOST` is the IP address or hostname of your proxy server and `PORT` is the port number.

## Logging

All log output is saved in the `log` directory, located in the same folder as the executable. Log file names follow the format:

```
list_import_20060102150405.log
```

## Troubleshooting

### Common Errors

- **`[ERROR] Error Decoding Configuration File:…`** — Typically caused by a missing quote (`"`) or comma (`,`) in `conf.json`. Use an online JSON validator to locate the issue.
- **`[ERROR] Database Query Error: read tcp … wsarecv: An established connection was aborted…`** — Most likely caused by incorrect credentials in the Keysafe entry referenced by `AppDBConf.KeysafeKeyID`. Check the username and password in that entry.
- **`[ERROR] Database Query Error: driver: bad connection.`** — As above; verify the database credentials in your Keysafe entry.

### Error Codes

| Code | Description |
|------|-------------|
| 100 | Unable to create log file |
| 101 | Unable to create log folder |
| 102 | Unable to load configuration file |
| 103 | Error processing authentication details |

## Change Log

### v3.0.0 - 2026

See release notes on GitHub.

### v2.0.0 - 2025

Initial release
