# Importing users from cloud services

## Overview

Going beyond the provision of on-premise hosted utilities for importing user objects and related records, Hornbill also provides the ability to import user objects natively from various cloud data sources.

This feature enables seamless data imports into Hornbill instances from external cloud services, eliminating the need for customer-hosted binaries or configuration. The import process for User type records is streamlined, while ensuring flexibility, security, and ease of management.

## Accessing Cloud Data Imports

Access to Cloud Data Imports is controlled by a system role called *Data Imports Administrator*. This role allows users to create and manage data-import configurations, and to run cloud-to-cloud data-import jobs.

To find Cloud Data Imports, navigate to **Configuration > Platform Configuration > Data > Cloud Data Imports**.

## How many separate imports will you do?
If you have both Full Users and Basic Users, you will likely want to do two imports. This is because all users brought in from a single import get assigned the same user type. You cannot import all of your users in one import and then afterward specify that some of them are are Full Users and others are Basic Users. You will need to do *more* than two imports if you'll have more than two user types. Plan your separate import jobs based on how many different groups you will organize your users into.

## Setting up a KeySafe key

To securely connect Hornbill to the cloud platform from which you're importing data, you’ll first need to create something called a *Hornbill KeySafe key*.

This key safely stores your login credentials and gives Hornbill permission to access the user data in your cloud platform.

- Depending on your cloud platform, use either the *Entra ID User Import* key type or the *Google Workspace User Management* key type.
- For help creating the key, see the [Platform Configuration Guide](/esp-config/security/keysafe).

Once your KeySafe Key is ready and connected to your cloud platform account, you’re all set to move on.

## Using the correct platform-specific import instructions

Cloud data-import configurations, although similar in many ways, do have target platform-specific implementations. Make sure to view the instructions for the platform you wish to pull data from:

- [Importing users from Entra ID](/data-imports-guide/cloud-users/entraid)
- [Importing users from Google Workspace](/data-imports-guide/cloud-users/googleworkspace)

But before you begin configuring your import, read on for more overview information that applies to all cloud imports regardless of data platform.

## Previewing an import

Once you have built your import configuration, before running or scheduling an import,  make sure to first run one or more preview import jobs. A preview import job returns a maximum of 50 records for review.

The next step is to review these records from the [Processing History](#processing-history) tab. Check that the job runs as intended --- that your user data is brought in the way you want. If not, run more preview import jobs until you get it right.

::: tip
You can change the default number of records returned in a data preview. For example, to diagnose a problem, you may want to run a preview for just a single user. In this case, you would set the data preview for just one record number (in **Run Import** > **Data Preview**), for example "From 1,234 to 1,234".
:::

Once you are happy with the import configuration, mappings, and records output from the preview, you can then either:

- Run an ad-hoc full import by clicking the **Run Import** button to the top right of the view.
- Schedule an import to run one or more times, by visiting the **Control & Schedule** tab.

## Viewing the processing history

The **Processing History** tab contains a list of all data-import jobs that have been executed using the selected data import configuration. You can see the run date; whether the run was a data preview, an impact analysis, or an actual import; the records processed and actioned; any problems encountered; and the job status (Initialized, Active, Completed, Succeeded, Failed).

::: note
A status of *Completed* means that, while the import did not fail, you need to check the processing history, because something didn't work as intended.
:::

**To view the processing history for an import job:**
1. In the **Configuration > Platform Configuration > Data > Cloud Data Imports**, in the list of imports that have run, find the job you want and click its name.
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

## Mapping and manipulating your user data
As an admin preparing for an import from a cloud data platform, make sure you are familiar with the ways you can [map and manipulate the user data](/data-imports-guide/cloud-users/data-mapping).