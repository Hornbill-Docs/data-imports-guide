# Running an Import

## Execute
### Command Line Parameters
- file - Defaults to 'conf.json' - Name of the Configuration file to load.
- dryrun - Defaults to 'false' - Set to true and the XMLMC for the creation and update of assets will not be called, and instead the generated XML for each asset will be dumped to the log file. This is to aid in debugging the initial connection information.
- creds - Defaults to 'false' - Set to true to return the API key that is stored in your import configuration. See NOTE in First Run, below. As well as user & computer specific access, you will also be prompted to input the ID of the instance that the API key was generated on.
- version - Defaults to 'false' - Set to true to return the version number of the tool, then exit

#### **First Run**

From version **2.0.0** of this utility, when you first run the utility it will prompt you for the ID of your Hornbill instance (case-sensitive), and the API key (also case-sensitive) that will be used to authenticate the API calls back into Hornbill. This information will be encrypted, and stored locally on the client PC that will be running the tool. For each subsequent import run, the utility will decrypt your instance ID and API key, and will use those to make the API calls back into Hornbill.

NOTE - the encrypted instance ID and API key can only be decrypted on the computer, and by the user, that performed the encryption, so please keep this in mind when scheduling your imports.

Should you wish to use a different API key to what has been previously encrypted, just delete the **import.cfg** file from the folder where the import binary resides, and re-run your import from the command line inputting the Instance ID and new API Key as you would have on its first run.

## Testing

If you run the application with the argument dryrun=true then no asset relationships will be created or updated, the XML used to create or update will be saved in the log file so you can ensure the data mappings are correct before running the import.

``asset_rel_import.exe -dryrun=true``