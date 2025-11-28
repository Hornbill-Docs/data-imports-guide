# Introduction

## Overview

The **Asset Import** utility provides Hornbill customers with a safe and secure way to manage asset records within the Service Manager application on the Hornbill platform, by synchronizing with records held in various different data sources.

The utility is designed to run behind a corporate firewall, connect to your database to query the required information, before transforming and loading the resulting data into your Hornbill instance as new or updated asset records. It supports both the initial bulk import as well as incremental adds and updates. You can schedule the tool to run periodically to perform the import/update tasks as required.

## Download

The utility can be downloaded from [GitHub](https://github.com/hornbill/asset-import/releases/latest). Please ensure to download the latest version of the ZIP archive that is relevant to the architecture of the computer it will be executed and scheduled to run on.

## Installation

Once downloaded, the ZIP archive for the utility should be extracted into a new folder that the user who will run the tool has read-write access to - whether that be your user profile to manually run the utility, the profile of a service account that will be used to schedule the running of it, or another folder that is accessible by the running account (domain user, local user, local system account or global managed system account). For example:

`C:\Users\YourUserOrServiceAccountID\Hornbill\asset_import`

## Updates

If the feature is enabled, the utility will self-update when it detects that a newer minor or patch version has been released on GitHub. All of the import tools use [semantic versioning](https://semver.org/), where:

* Major version updates will contain new feature(s), and include breaking changes to the tool configuration 
* Minor version updates will contain new feature(s) and do not include breaking changes to the tool configuration
* Patch version updates will contain bug fixes only 

When there is a Major version update, the utility will not self-update but instead will output a message to the command line & log(s) stating that there is a newer version available to download from GitHub. In this instance, you should download the [latest release from GitHub](https://github.com/hornbill/asset-import/releases/latest), and review the breaking configuration changes against your existing import configuration before implementation. You can also check the [Hornbill Community Forum](https://community.hornbill.com/forum/135-announcements/) for real-time release updates.

## Requirements 

[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/requirements.md]]