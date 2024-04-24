# Running an Import

Ultimately, the executable will be scheduled in the Windows task scheduler (see later) but to test, gain confidence, and perform an initial upload of users, the utility can be executed from a command prompt window on an ad-hoc basis. The command used to execute the import can contain several command line arguments.

## Command Line Arguments

- `creds` - Defaults to `false` - Set to `true` to decrypt and output the API key that is stored locally. The utility will prompt you for your Instance ID, and this can only be decrypted by the user who originally performed the encryption, and only on the same machine that it was encrypted on.
- `debug` - Defaults to `false` - Set to `true` to output extra information to the log to aid in debugging issues.
- `dryrun` - Defaults to `false` - Set to `true` and the API calls to create and update users will not be run. Instead, the API call request payloads will be output to the log file to aid in debugging.
- `file` - Defaults to `conf.json` - The name of the import configuration file to use.
- `forcerun` - Defaults to `false` - Set to `true` to to bypass the existing running job check. This should be used if a previous run did not successfully complete and the utility is reporting that an import is in progress.
- `workers` - Defaults to `3` - Allows you to change the number of worker threads used to process the import; increasing this can improve performance on slow import but using too many workers can have a detriment to the performance of your Hornbill instance while the import is running.

:::warning
The `forcerun` argument should **ONLY** be used when you are certain that another import is not actively in progress. Running multiple imports simultaneously against the same User records can have unexpected, and likely undesired, effects on the imported data.
:::

## First Run

When you first run the utility it will prompt you for two vital pieces of information:

- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access the Hornbill service:
  - ht<span>tps://live.hornbill.com/</span>`instanceid` (case sensitive).
- A valid API key. This needs to be created against a Hornbill user account with enough rights to create and update user accounts. Details on how to create an API key can be found in the [Hornbill Platform Fundamentals](/esp-fundamentals/security/api-keys) book.

This information will be encrypted and stored locally on the host computer that will run the utility. For each subsequent import run, the utility will decrypt your instance ID and API key from the client and will use those to make the API calls back into Hornbill as necessary to perform the import.

::: important 
The authentication information can only be decrypted on the computer (physical or virtual) that the encryption was performed on, and by the user that performed the encryption, so please bear this in mind when scheduling your imports.
:::

### First Run Example

The following is an example of how the tool will prompt you for your Instance ID and API Key on the first run of the utility:

<img src="/_books/data-imports-guide/users/azure/images/az_user_import.png" width="800px" alt="First Run Example"/>

## Testing Overview

There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities. The User Import - Azure utility accepts and understands a number of [command line arguments](/data-imports-guide/users/azure/command#command-line-arguments) that can be used when running the utility from the Windows command line or PowerShell. 

:::tip
The most important argument when testing is `-dryrun=true`. When this is provided, no user create or update requests will be sent to Hornbill. Instead, all create or update requests that would have been made into Hornbill are output to a log file, including input parameters, which provides you with an opportunity to review their content and understand any error messages that may occur.
:::

Below are some high level steps to help you build confidence in your configuration:

1. In the configuration, specify a `UserFilter` to target a single user object. It's good practice to initially test against a single, or small set of, user objects, as this allows the dryrun to complete quicker and there is less log content to review.
1. Perform a dryrun (by executing the utility along with the `-dryrun=true` command line parameter) - this allows you to confirm that the configuration is correct and that a connection to Azure can be established, without importing any user data into your Hornbill instance. 
1. Review the commind line output and log file for any errors or warnings, and where necessary check and rectify any errors against the Common Error Messages listed in the [Troubleshooting Azure User Imports](/data-imports-guide/users/azure/debugging) section of this book.
1. Continue with dryrun tests until you are happy that all the errors are accounted for.
1. Perform a live import against a single user object (set `-dryrun=false`).
1. Review the imported user account in Hornbill and check all user properties are as expected i.e. email contains an email address etc.
1. Where necessary, adjust the configuration file user property mappings
1. Loop through steps 5 - 7 as many times as is necessary until you are happy with the information being transported into the Hornbill user account properties.
1. Amend the `UserFilter` and/or the scope of the `Search` variable to target the user objects required for a full import.
1. Perform a dryrun
1. Review the commind line output and log file for any errors or warnings, and where necessary check and rectify any errors against Common Error Messages listed in the [Troubleshooting Azure User Imports](/data-imports-guide/users/azure/debugging) section of this book.
1. Continue with dryrun tests until you are happy that all the errors are accounted for.

## Command Line Examples

```cmd
azure_user_import.exe -file="my_config.json" -dryrun=true
```

```powershell
.\azure_user_import.exe -file="my_config.json" -dryrun=true
```

## Resetting Encrypted Credentials

Should you wish to use a different Instance ID or API key to what has been previously encrypted, then simply delete the `import.cfg` file from the folder where the import binary resides. Once you re-run your import from the command line, the utility will again ask for the Instance ID and API Key details just as it would have on its first run.