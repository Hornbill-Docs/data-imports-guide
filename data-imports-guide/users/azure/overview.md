# Introduction

## Overview

The User Import - Azure utility provides Hornbill customers with a safe and secure way to manage user accounts on the Hornbill platform by synchronizing with accounts held in Entra ID (formerly known as Azure AD).

The utility is designed to run behind a corporate firewall, connect to Entra ID and query the required information, before transforming and loading the resulting data into your Hornbill instance as new or updated user records. 

The utility connects to the Hornbill instance and Azure tenant over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the utility without the need to make any firewall configuration changes. The tool supports both the initial bulk import as well as incremental adds and updates. You can schedule the tool to run periodically to perform the import/update tasks as required.

## Download

The User Import - Azure utility can be downloaded from [GitHub](https://github.com/hornbill/user-import-azure/releases/latest). Please ensure to download the latest version of the ZIP archive which is relevant to the architecture of the computer it will be run on.

## Installation

Once downloaded, the ZIP archive for the utility should be extracted into a new folder within the local user profile of the user who will run the tool - whether that be your user profile to manually run the utility or the profile of a service account that will be used to schedule the running of it. For example:

`C:\Users\YourUserOrServiceAccountID\Hornbill\user_import_azure`

## Updates

The User Import - Azure utility will self-update when it detects that a newer minor or patch version has been released on GitHub. All of the import tools use [semantic versioning](https://semver.org/), where:

* Major version updates will contain new feature(s), and include breaking changes to the tool configuration 
* Minor version updates will contain new feature(s) and do not include breaking changes to the tool configuration
* Patch version updates will contain bug fixes only 

When there is a Major version update, the utility will not self-update but instead will output a message to the command line & log(s) stating that there is a newer version available to download from GitHub. In this instance, you should download the [latest release from GitHub](https://github.com/hornbill/user-import-azure/releases/latest), and review the breaking configuration changes against your existing import configuration before implementation.

## Requirements 

### Host

The program that performs the import is fairly lightweight and doesn't require much in the way of hardware to run. It can be run on virtualized or physical hardware running any version of Windows currently supported by Microsoft, but basic guidelines are as follows:

* Operating System - Microsoft Windows, 32 or 64-bit, current/LTS, desktop/server
* CPU - One or more cores
* RAM - 4GB minimum 

### Network

The utility connects to the Hornbill and Azure instances in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use it without the need to make any proxy or firewall configuration changes.

#### HTTP Proxies

If you use a proxy for your internet traffic, the HTTP_PROXY and HTTPS_PROXY environment variables will need to be set. These environment variables hold the hostname or IP address of your proxy server. The proxy environment variables can be set from a command line as so:

```cmd
set HTTP_PROXY=HOST:PORT
set HTTPS_PROXY=HOST:PORT
```

Where "HOST" is the IP address or hostname of your Proxy Server and "PORT" is the specific port number. If you require a username and password to go through the proxy, the format for the setting is as follows:

```cmd
set HTTP_PROXY=username:password@HOST:PORT
set HTTPS_PROXY=username:password@HOST:PORT
```

#### URL White Listing

Occasionally, on top of setting the HTTP_PROXY and HTTPS_PROXY variables, the following URLs may need to be white-listed to allow access out from your network to the required endpoints in the Hornbill network:

* `https://*.hornbill.com/*` - allows access to the required Hornbill instance information and API endpoints
* `https://api.github.com/repos/hornbill/*` - allows the utility to self-update
* `https://login.microsoftonline.com/*` - allows access generate access tokens for Azure, to cache your defined Azure information
* `https://graph.microsoft.com/*` - allows access to cache your defined Azure information