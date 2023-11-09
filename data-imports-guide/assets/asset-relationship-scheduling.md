# Scheduling an Import

## Windows
You can schedule **The Asset Relationship Import / asset_rel_import.exe** to run with any optional command line argument from Windows Task Scheduler:

There are several caveats that should be considered when scheduling this utility using the Windows Task Scheduler:

- Ensure the user account running the task has rights to asset_rel_import.exe and the containing folder. This is catered for automatically by Windows if the utility is stored and run from the service account user profile folder, as per the [installation instructions](/data-imports-guide/assets/asset-relationship-intro).
- Make sure the ``Start In`` parameter contains the folder where asset_rel_import.exe resides in otherwise it will not be able to pick up the correct path.

:::warning
If you do not have the value of `Start In` set to the folder where the executable resides, then the task will not complete and will eventually time out in your scheduler. This is because the import utility cannot detect the file that contains your instance credentials, and is interactively prompting for these to be provided.
:::