# Importing users from cloud services

## Overview

Going beyond the provision of on-premise hosted utilities for importing user objects and related records, Hornbill also provides the ability to import user objects natively from various cloud data sources.

This feature enables seamless data imports into Hornbill instances from external cloud services, eliminating the need for customer-hosted binaries or configuration. The import process for user-type records is streamlined, while ensuring flexibility, security, and ease of management.

When importing users from cloud services, you will generally take the following steps:
1. (One time only) If you have not yet imported data from cloud services, [you must set up a KeySafe key](/data-imports-guide/cloud-users/overview#setting-up-a-keysafe-key).
1. Read [Data Mapping](/data-imports-guide/cloud-users/data-mapping) to understand how you can manipulate the user data as part of setting up your import configuration. You need to understand that you'll be mapping your source data using Mustache tags, and that you can make changes to the data as you bring it in by applying transformations.
1. Set up an import configuration. This varies based on your cloud platform. See the instructions [for Entra ID](/data-imports-guide/cloud-users/entraid) or [for Google Workspace](/data-imports-guide/cloud-users/googleworkspace), depending on your platform.
1. [Run a preview of the import](/data-imports-guide/cloud-users/overview#previewing-an-import). Make edits to your configuration until you're happy with how the data looks.
1. [Run an impact analysis](/data-imports-guide/cloud-users/overview#running-an-impact-analysis). Again, make edits to the configuration until you're happy.
1. Run the import using the **Run Import** option, which is when the users actually get imported. Another option is to [schedule the import run](/data-imports-guide/cloud-users/overview#scheduling-an-import).

## Accessing Cloud Data Imports

Access to Cloud Data Imports is controlled by a system role called *Data Imports Administrator*. This role allows users to create and manage data-import configurations, and to run cloud-to-cloud data-import jobs.

To find Cloud Data Imports, navigate to **Configuration > Platform Configuration > Data > Cloud Data Imports**.

## Setting up a KeySafe key

To securely connect Hornbill to the cloud platform from which you're importing data, you’ll first need to create something called a *Hornbill KeySafe key*.

This key safely stores your login credentials and gives Hornbill permission to access the user data in your cloud platform.

- Depending on your cloud platform, use either the *Entra ID User Import* key type or the *Google Workspace User Management* key type.
- For help creating the key, see the [Platform Configuration Guide](/esp-config/security/keysafe).

Once your KeySafe key is ready and connected to your cloud platform account, you’re all set to move on.

## How many separate imports will you do?
If you have both Full Users and Basic Users, you will likely want to do two imports. This is because all users brought in from a single import get assigned the same user type. You cannot import all of your users in one import and then later specify that some of them are are Full Users and others are Basic Users. You will need to do *more* than two imports if you'll have more than two user types. Plan your separate import jobs based on how many different groups you will organize your users into.

## Using the correct platform-specific import instructions

Cloud data-import configurations, although similar in many ways, do have target platform-specific implementations. Make sure to view the instructions for the platform you wish to pull data from:

- [Importing users from Entra ID](/data-imports-guide/cloud-users/entraid)
- [Importing users from Google Workspace](/data-imports-guide/cloud-users/googleworkspace)

But before you begin configuring your import, read on for more overview information that applies to all cloud imports regardless of data platform.

## Previewing an import
Once you have built your import configuration, before running or scheduling an import, make sure to first run one or more preview jobs.

When you click the **Run Import** button, you'll see there are three ways to run an import: **Data Preview**, **Analyze Impact**, and the actual import itself. The first two allow you to make sure that your import configuration will work as intended before you run the actual import.

![Data Preview, Impact Analysis, Run Import](/_books/data-imports-guide/cloud-users/images/cloud-import-run-import.png)

When you run a data preview, by default, you get the first 50 records for review. You review these records from the [Processing History](/data-imports-guide/cloud-users/overview#about-viewing-the-processing-history) tab. Check that the job runs as intended --- that your user data is brought in the way you want. If not, make changes to your configuration and then run more preview jobs until you get it right.

::: tip
You can change the default number of records returned in a data preview. For example, to diagnose a problem, you may want to run a preview for just a single user. In this case, you would set the data preview for just one record number (in **Run Import** > **Data Preview**), for example "From 1,234 to 1,234".
:::

**To run a preview for an import configuration:**
1. In **Configuration > Platform Configuration > Data > Cloud Data Imports**, in the list of imports, find the config you want and click its name.
1. In any of the tabs, click **Run Import > Data Preview**.
1. Click the Processing History tab, and then the date for the the most recent run.
1. In the Details tab, you can view the processing information.
    * The Details tab provides information about the import run as well as a summary of counts for records added and updated, new users added, roles granted, groups assigned, and any problems encountered.
    * The Log tab provides detailed information about each step of the run in chronological order. You can filter what you see in the log by source (System, iBridge, or Import) and by severity (Error, Warning, Notice, Info).
    * The Records tab provides information about each of the records returned in the preview. Do a spot-test by checking through a few records.
        ::: note
        If no records have been returned, the Records tab does not appear.
        :::
1. (Optional) If your data preview reveals problems in the import configuration, use the **Include Diagnostics** toggle and run another preview. This option provides greater detail in the Log tab about what happened in the run.

## Running an impact analysis
When you use the **Analyze Impact** option in the **Run Import** dropdown, the system runs through your import configuration and tells you the changes to be made, without actually making the changes. This way you can verify that the changes are as expected.

**To run an impact analysis for an import configuration:**
1. In **Configuration > Platform Configuration > Data > Cloud Data Imports**, in the list of imports, find the config you want and click its name.
1. In any of the tabs, click **Run Import > Analyze Impact**.
1. Click the Processing History tab, and then the date for the the most recent run.
1. In the Processing Information view, you can view the impact analysis.
    * The Details tab provides information about the import run as well as a summary of counts for records added and updated, new users added, roles granted, groups assigned, and any problems encountered.
    * The Log tab provides detailed information about each step of the run in chronological order. You can filter what you see in the log by source (System, iBridge, or Import) and by severity (Error, Warning, Notice, Info).
1. (Optional) If your impact analysis reveals problems in the import configuration, use the **Include Diagnostics** toggle and run another impact analysis. This option provides greater detail in the Log tab about what happened in the run.
-->
### About viewing the processing history

The **Processing History** tab contains a list of all data-import jobs that have been executed using the selected data import configuration. You can see the run date; whether the run was a data preview, an impact analysis, or an actual import; the records processed and actioned; any problems encountered; and the job status (*Initialized*, *Active*, *Completed*, *Succeeded*, *Failed*).

You cannot run more than one import at once for any one import configuration. If you see a status of *Initialized*, this means the system is waiting for the first run to finish before beginning the next.

A status of *Completed* means that, while the import did not fail, you need to check the processing history because something didn't work as intended.

**To view the processing history for an import job:**
1. In **Configuration > Platform Configuration > Data > Cloud Data Imports**, in the list of imports that have run, find the job you want and click its name.
1. In the Details tab, view the import details for the selected job. Here you can see when the import configuration was created and/or updated and by whom.
1. (Optional) Click the **Import Enabled** toggle to disable the import configuration.

<!--This view contains one or two tabs:
- **Details.** Contains useful information pertaining to the job, as well as a log viewer so that you can see exactly what the import job did, plus any debugging information should the import have failed.
- **Records.** Allows you to view the import preview records. Only visible when:
    - The user import job was run in **Preview** mode.

    - There are one or more records that have been returned by the data-import connector.-->
<!-- CAMMY, make this visible or move to more to-do tasks:
1. (Optional) In the Data Source tab, _____.
1. (Optional) In the Control & Schedule tab, ... **View Log**.
1. (Optional) In the Processing History tab, ...-->

## Scheduling an import
Once you are happy with the import configuration you have set up, its mappings, and the records output from your data previews, you can then run an ad-hoc full import by using the **Run Import** button. There is, however, another option for running imports: you can put them on a schedule.

**To schedule an import to run one or more times:**
1. In **Configuration > Platform Configuration > Data > Cloud Data Imports**, in the list of imports, find the configuration you want and click its name.
1. Click the **Control & Schedule** tab.
1. Set up your schedule as required, specifying recurrence, time zone, next run date, days of the week, and number of executions before expiring.
1. (Optional) If the schedule has previously run, click **View Log** to see its history.
1. Click **Save & Run**.