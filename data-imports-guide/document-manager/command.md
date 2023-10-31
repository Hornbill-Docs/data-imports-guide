# Running an Import

Ultimately, the executable will be scheduled in the Windows task scheduler (see later) but to test, gain confidence, and perform an initial upload of asset records into Document Manager, the utility can be executed from a command prompt window on an ad-hoc basis. The command used to execute the import can contain several command line arguments.

## Command Line Parameters

- `apikey`- API Key to use as Authentication when connecting to Hornbill Instance
- `apitimeout`- Number of Seconds to Timeout an API Connection (default 60)
- `csvc`- Name of the CSV file containing document collection data
- `csvd`- Name of the CSV file containing main document data
- `csvs`- Name of the CSV file containing document sharing data
- `csvt`- Name of the CSV file containing document tag data
- `debug`- true/false, defaults to false - Log extended debug information
- `dryrun`- true/false, defaults to false - Allow the Import to run without Creating Documents
- `instanceid`- This is the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.`hornbill.com/[instance name]/`. E.g. if the URL you use to access your instance is `live.hornbill.com/arescomputing/`, then your instance id would be "arescomputing". This value is case sensitive.
- `version`- Output the tools version number, and exit

## First Run
When you first run the utility it will prompt you for two vital pieces of information:

- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access the Hornbill service:
    - https://live.hornbill.com/instanceid (case sensitive).
- A valid [API key](https://docs-internal.hornbill.com/data-imports-guide/assets/authentication#api-keys). This needs to be created against a Hornbill user account with enough rights to create and update asset records in Document Manager. Details on how to create an API key can be found in the [Hornbill Platform Fundamentals](https://docs-internal.hornbill.com/esp-fundamentals/security/api-keys) book.
This information will be encrypted and stored locally on the host computer that will run the utility. For each subsequent import run, the utility will decrypt your instance ID and API key from the client and will use those to make the API calls back into Hornbill as necessary to perform the import.

:::important
The authentication information can only be decrypted on the computer (physical or virtual) that the encryption was performed on, and by the user that performed the encryption, so please bear this in mind when scheduling your imports.
:::

## Testing Overview

There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities. The Asset Import utility accepts and understands a number of [command line arguments](https://docs-internal.hornbill.com/data-imports-guide/document-manager/command#command-line-arguments) that can be used when running the utility from the Windows command line or PowerShell.

:::tip
The most important argument when testing is `-dryrun=true`. When this is provided, no documents will be sent to Hornbill. Instead, all import requests that would have been made into Hornbill are output to a log file, including input parameters, which provides you with an opportunity to review their content and understand any error messages that may occur.
:::

Below are some high level steps to help you build confidence in your configuration:

1. In the configuration, write your query accordingly to target a single or small set of document records. It’s good practice to initially test against a single, or small set of, objects, as this allows the dryrun to complete quicker and there is less log content to review.
2. Perform a dryrun (by executing the utility along with the `-dryrun=true` command line parameter) - this allows you to confirm that the configuration is correct and that a connection to the source database can be established, without importing any asset data into your Hornbill instance.
3. Review the commind line output and log file for any errors or warnings, and where necessary check and rectify any errors against the Common Error Messages listed in the [Troubleshooting Document Imports](/data-imports-guide/document-manager/debugging) section of this book.
4. Continue with dryrun tests until you are happy that all the errors are accounted for.
5. Perform a live import against a single asset record (set `-dryrun=false`).
6. Review the imported document record in Document Manager, and check all properties are as expected i.e. Document tag contains a valid Document tag etc.
7. Where necessary, adjust the configuration file asset property mappings
8. Loop through steps 5 - 7 as many times as is necessary until you are happy with the information being transported into the Document Manager document record properties.
9. Amend the query to target the asset objects required for a full import.
10. Perform a dryrun
11. Review the commind line output and log file for any errors or warnings, and where necessary check and rectify any errors against Common Error Messages listed in the [Troubleshooting Document Imports](/data-imports-guide/document-manager/debugging) section of this book.
12. Continue with dryrun tests until you are happy that all the errors are accounted for.

## Command Line Examples

```cmd
goHornbillDocumentImport.exe -dryrun=true
```

```powershell
.\goHornbillDocumentImport.exe - dryrun=true
```

## Resetting Encrypted Credentials
Should you wish to use a different Instance ID or API key to what has been previously encrypted, then simply delete the `import.cfg` file from the folder where the import binary resides. Once you re-run your import from the command line, the utility will again ask for the Instance ID and API Key details just as it would have on its first run.