# Running an Import

Ultimately, the executable will be scheduled in the Windows task scheduler (see later) but to test, gain confidence, and perform an initial upload of asset relationship records into Service Manager, the utility can be executed from a command prompt window on an ad-hoc basis. The command used to execute the import can contain several command line arguments.

## Command Line Arguments

- `creds` - Defaults to `false` - Set to true to return the API key that is stored in your import configuration. See NOTE in First Run, below. As well as user & computer specific access, you will also be prompted to input the ID of the instance that the API key was generated on.
- `file` - Defaults to `conf.json` - The name of the import configuration file to use.
- `dryrun` - Defaults to `false` - Set to `true` and the API calls to create and update asset relationship records will not be run. Instead, the API call request payloads will be output to the log file to aid in debugging.
- `version` - Defaults to `false` - Set to true to return the version number of the tool, then exit.

## First Run

When you first run the utility it will prompt you for two vital pieces of information:

- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access the Hornbill service:
  - ht<span>tps://live.hornbill.com/</span>`instanceid` (case sensitive).
- A valid [API key](/data-imports-guide/asset-relationships/authentication#api-keys). This needs to be created against a Hornbill user account with enough rights to create and update asset records in Service Manager. Details on how to create an API key can be found in the [Hornbill Platform Fundamentals](/esp-fundamentals/security/api-keys) book.

This information will be encrypted and stored locally on the host computer that will run the utility. For each subsequent import run, the utility will decrypt your instance ID and API key from the client and will use those to make the API calls back into Hornbill as necessary to perform the import.

::: important 
The authentication information can only be decrypted on the computer (physical or virtual) that the encryption was performed on, and by the user that performed the encryption, so please bear this in mind when scheduling your imports.
:::

## Testing Overview

There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities. The Asset Relationship Import utility accepts and understands a number of [command line arguments]/data-imports-guide/asset-relationships/command#command-line-arguments) that can be used when running the utility from the Windows command line or PowerShell.

:::tip
The most important argument when testing is -dryrun=true . When this is provided, no record create or update API calls will be sent to Hornbill. Instead, all create or update requests that would have been made into Hornbill are output to a log file, including input parameters, which provides you with an opportunity to review their content and understand any error messages that may occur.
:::

Below are some high level steps to help you build confidence in your configuration:
1. In the configuration, write your query accordingly to target a single or small set of asset relationship records. It’s good practice to initially test against a single, or small set of, objects, as this allows the dryrun to complete quicker and there is less log content to review.
2. Perform a dryrun (by executing the utility along with the `-dryrun=true` command line parameter) - this allows you to confirm that the configuration is correct and that a connection to the source database can be established, without importing any asset relationship data into your Hornbill instance.
3. Review the commind line output and log file for any errors or warnings, and where necessary check and rectify any errors against the Common Error Messages listed in the [Troubleshooting](/data-imports-guide/asset-relationships/debugging) section of this book.
4. Continue with dryrun tests until you are happy that all the errors are accounted for.
5. Perform a live import against a single asset relationship record (set `-dryrun=false`).
6. Review the imported asset relationship record in Service Manager, and check the relationship exists against the correct asset records.
7. Where necessary, adjust the configuration file relationship properties and re-run.
8. Loop through steps 5 - 7 as many times as is necessary until you are happy with the information being transported into the Service Manager asset record properties.
9. Amend the query to target the asset objects required for a full import.
10. Perform a dryrun.
11. Review the command line output and log file for any errors or warnings, and where necessary check and rectify any errors against Common Error Messages listed in the [Troubleshooting](/data-imports-guide/asset-relationships/debugging) section of this book.
12. Continue with dryrun tests until you are happy that all the errors are accounted for.

## Command Line Examples

```cmd
asset_rel_import.exe -file="my_config.json" -dryrun=true
```
```powershell
asset_rel_import.exe -file="my_config.json" -dryrun=true
```

## Resetting Encrypted Credentials

Should you wish to use a different Instance ID or API key to what has been previously encrypted, then simply delete the `import.cfg` file from the folder where the import binary resides. Once you re-run your import from the command line, the utility will again ask for the Instance ID and API Key details just as it would have on its first run.