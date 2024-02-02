# Scheduling an Import

## Windows
You can schedule the **Asset Relationship Import** utility to run with any of the documented command line arguments, from within the Windows Task Scheduler.

:::important
Do not schedule your imports to run during the instance update window, as the imports may fail or act in an unexpected manner if the instance installs updates during this time.

The update windows can be found in the [Continuous Delivery](/hornbill-cloud/continuous-delivery#hornbill-update-deployment-process) document.
:::

There are several caveats that should be considered when scheduling this utility using the Windows Task Scheduler:

- Ensure the user account running the task has rights to the import executable and the containing folder. This is catered for automatically by Windows if the utility is stored and run from the service account user profile folder, as per the [installation instructions](/data-imports-guide/asset-relationships/overview#installation).
- Ensure the user account running the task is the one who performed the first run of the tool on the host computer, as per the [first run instructions](/data-imports-guide/asset-relationships/command#first-run).
- Make sure the `Start In` parameter contains the folder where the executable resides.

:::warning
If you do not have the value of `Start In` set to the folder where the executable resides, then the task will not complete and will eventually time out in your scheduler. This is because the import utility cannot detect the file that contains your instance credentials, and is interactively prompting for these to be provided.
:::