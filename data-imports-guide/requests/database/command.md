# Running an Import

## Command Line Parameters

- file - Defaults to 'conf.json' - Name of the Configuration file to load
- dryrun - Defaults to `false` - Set to True and the XMLMC for the raising of new requests will not be called, and instead the generated XML for each request will be dumped to the log file. This is to aid in debugging the initial connection information.

First Run
When you first run the utility it will prompt you for two vital pieces of information:

- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access the Hornbill service:
    - `https://live.hornbill.com/instanceid` (case sensitive).
- A valid [API key](https://docs-internal.hornbill.com/data-imports-guide/assets/authentication#api-keys). This needs to be created against a Hornbill user account with enough rights to create and update asset records in Service Manager. Details on how to create an API key can be found in the [Hornbill Platform Fundamentals](https://docs-internal.hornbill.com/esp-fundamentals/security/api-keys) book.
This information will be encrypted and stored locally on the host computer that will run the utility. For each subsequent import run, the utility will decrypt your instance ID and API key from the client and will use those to make the API calls back into Hornbill as necessary to perform the import.

:::important
The authentication information can only be decrypted on the computer (physical or virtual) that the encryption was performed on, and by the user that performed the encryption, so please bear this in mind when scheduling your imports.
:::

## Testing Overview
There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities. The Asset Import utility accepts and understands a number of [command line arguments](https://docs-internal.hornbill.com/data-imports-guide/requests/database/command#command-line-arguments) that can be used when running the utility from the Windows command line or PowerShell.

:::tip
The most important argument when testing is` -dryrun=true`. When this is provided, no record create or update requests will be sent to Hornbill. Instead, all create or update requests that would have been made into Hornbill are output to a log file, including input parameters, which provides you with an opportunity to review their content and understand any error messages that may occur.
:::