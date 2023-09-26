# Scheduling

You can schedule the **User Import - LDAP** utility to run, with any of the documented command line arguments, from within the Windows Task Scheduler.

There are several caveats that should be considered when scheduling this utility using the Windows Task Scheduler:

- Ensure the user account running the task has rights to the import executable, configuration file and the containing folder. This is catered for automatically by Windows if the utility is stored and run from the service account user profile folder, as per the [installation instructions](/data-imports-guide/users/ldap/overview#installation).
- Ensure the user account running the task is the one who performed the first run of the tool on the host computer, as per the [first run instructions](/data-imports-guide/users/ldap/command#first-run).
- Make sure the `Start In` parameter contains the folder where the executable resides.