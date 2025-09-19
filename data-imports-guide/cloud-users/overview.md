# Importing Users from Cloud Services

## Overview

Going beyond the provision of on-premise hosted utilities for importing user objects and related records, Hornbill also provides the ability to import user objects natively from various cloud data sources.

This feature enables seamless data imports into Hornbill instances from external cloud services, eliminating the need for customer-hosted binaries or configuration. The import process for User type records is streamlined, while ensuring flexibility, security, and ease of management.

## Accessing Cloud Data Imports

Access to the Cloud Data Imports is controlled by a system role called `Data Imports Administrator`. This role allows uses to create and manage data-import configurations, and to run cloud-to-cloud data-import jobs.

## Configuring an Import

Cloud data import configurations, although similar in many ways, do have target platform-specific implementations, so please view the instructions for the platform you wish to pull data from:

- [Entra ID User Import](/data-imports-guide/cloud-users/entraid)
- [Google Workspace User Import](/data-imports-guide/cloud-users/googleworkspace)

## Running or Scheduling an Import

One you have built your cloud data import configuration, it is highly recommended that you first run one or more preview import jobs. A preview import job returns a maximum of 50 records for review. You then review these records from the [Import History](#import-history) tab. 

Once you are happy with the import configuration, mappings, and records output from the preview, you can then either:

- Run an ad-hoc full import by clicking the **Run Import** button to the top right of the view.
- Schedule an import to run one or more times, by visiting the **Control & Schedule** tab.

## Import History

The **Import History** tab contains a list of all data-import jobs that have been executed using the selected data import configuration, along with who created the job, whether or not it was defined as a preview, and the status of the job (`running` / `succeeded` / `failed`).

Clicking on the name of one of the import jobs will take you to the Import Details for the selected job. This view contains one or two tabs:

- `Details` - Contains useful information pertaining to the job, as well as a log viewer so that you can see exactly what the import job did, plus any debugging information should the import have failed.
- `Records` - Allows you to view the import preview records. Only visible when:
    - The user import job was run in **Preview** mode.
    - There are one or more records that have been returned by the data-import connector.