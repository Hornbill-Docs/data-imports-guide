# Running the Utility

Ultimately, the executable will be scheduled in the Windows task scheduler (see later) but to test, gain confidence, and perform an initial upload of users, the utility can be executed from a command prompt window on an ad-hoc basis. The command used to execute the import can contain several command line arguments.

## Command Line Arguments

- `creds` - Defaults to `false` - Set to `true` to decrypt and output the API key that is stored locally. The utility will prompt you for your Instance ID, and this can only be decrypted by the user who originally performed the encryption, and only on the same machine that it was encrypted on.
- `debug` - Defaults to `false` - Set to `true` to output extra information to the log to aid in debugging issues.
- `dryrun` - Defaults to `false` - Set to `true` and the API calls to create and update users will not be run. Instead, the API call request payloads will be output to the log file to aid in debugging.
- `file` - Defaults to `conf.json` - The name of the import configuration file to use.
- `workers` - Defaults to `3` - Allows you to change the number of worker threads used to process the import; increasing this can improve performance on slow import but using too many workers can have a detriment to the performance of your Hornbill instance while the import is running.

## First Run

When you first run the utility it will prompt you for two vital pieces of information:

- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access your Hornbill instance:
  - ht<span>tps://live.hornbill.com/</span>`instanceid` (case sensitive).
- A valid API key. This needs to be created against a Hornbill user account with enough rights to create and update user accounts. Details on how to create an API key can be found here.

This information will be encrypted and stored locally on the host computer that will run the utility. For each subsequent import run, the utility will decrypt your instance ID and API key from the client and will use those to make the API calls back into Hornbill that are necessary to perform the import.

::: important 
The authentication information can only be decrypted on the computer (physical or virtual) that the encryption was performed on, and by the user that performed the encryption, so please bear this in mind when scheduling your imports.
:::

## Command Line Examples

```cmd
azure_user_import.exe -file="my_config.json" -workers=5
```

```powershell
.\azure_user_import.exe -file="my_config.json" -workers=5
```

## Resetting Encrypted Credentials

Should you wish to use a different Instance ID or API key to what has been previously encrypted, then simply delete the `import.cfg` file from the folder where the import binary resides. Once you re-run your import from the command line, the utility will again ask for the Instance ID and API Key details just as it would have on its first run.