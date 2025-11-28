# Scheduling

You can schedule the **Asset Import** utility to run, with any of the documented command line arguments, from within the Windows Task Scheduler.

:::important
Do not schedule your imports to run during the instance update window, as the imports may fail or act in an unexpected manner if the instance installs updates during this time.

The update windows can be found in the [Continuous Delivery](/hornbill-cloud/continuous-delivery#hornbill-update-deployment-process) document.
:::

There are several caveats that should be considered when scheduling this utility using the Windows Task Scheduler:

- Ensure the user account running the task has rights to the import executable and the containing folder. This is catered for automatically by Windows if the utility is stored and run from the service account user profile folder, as per the [installation instructions](/data-imports-guide/assets/overview#installation).
- Ensure the user account running the task is the one who performed the first run of the tool on the host computer, as per the [first run instructions](/data-imports-guide/assets/command#first-run).
- Make sure the `Start In` parameter contains the folder where the executable resides.

:::warning
If you do not have the value of `Start In` set to the folder where the executable resides, then the task will not complete and will eventually time out in your scheduler. This is because the import utility cannot detect the file that contains your instance credentials, and is interactively prompting for these to be provided.
:::

## Using Group Managed Service Accounts (gMSA) when Scheduling

The Asset Import utility, from version `v4.16.0`, supports the use of gMSA accounts as the user account in your Windows Task Scheduler tasks. Due to the encryption implementation in the utility, and the fact that you cannot run interactive sessions under a gMSA account, you should follow the below when scheduling your imports:

- Create your Import Task in Task Scheduler, with the following configuration:
    - **General**
        - **When running the task, use the following user account**
            - Set this to the gMSA account you are using:
                - `your-domain\yourgmsaaccountid$`
        - ✅ **Run whether user is logged in or not**
    - **Actions > Start a Program**
        - **Program/Script**
            - The path to the `asset_import.exe` binary
        - **Add arguments**
            - This should be the arguments that you wish to use when running the import tool, but with the addition of `setupkey` and `setupinstance` arguments so that the utility cansave & encrypt the instance credentials against the gMSA:
                - `-file .\my_import_configuration.json -setupkey yourhornbillapikey -setupinstance yourhornbillinstanceid`
        - **Start in**
            - The path where your `asset_import.exe` binary resides
- Run the Import Task as a one-off. This will run the import, encrypting and storing the instance credentials for future Task runs
- Edit the Import Task to add your schedule/triggers:
    - Remove ` -setupkey yourhornbillapikey -setupinstance yourhornbillinstanceid` from the arguments parameter as they are no longer needed, and for security purposes you do not want to save Hornbill instance credentials in plain-text in your Tasks!
    - Add your Import triggers as required (schedule)
    - Re-add your gMSA account in the **When running the task, use the following user account** input. This is due to an issue with Task Scheduler, where it will incorrectly ask for your gMSA account password when saving changes to the Task unless the account is reselected in the Task UI
    - Save the Import Task
