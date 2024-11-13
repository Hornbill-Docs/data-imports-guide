# Introduction

## Overview
The **Hornbill Document Importer** provides a simple, safe and secure way to bulk-import documents into Hornbill Document Manager. The tool is designed to facilitate the initial upload of content to Hornbill Document Manager cater for the initial upload and therefore does not perform updates to existing Document Manager documents.

The tool is designed to run behind your corporate firewall and connects to your Hornbill instance in the cloud over HTTPS/SSL. So as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

## Installation

Once downoaded, the [Zip archive](https://github.com/hornbill/goHornbillDocumentImport/releases/latest) for the utility should be extracted into a new folder within the local user profile of the user who will run the tool - whether that be your user profile of a service account that will be used to schedule the running of it.
For example:
`C:\Users\YourUserOrServiceAccountID\Hornbill\docimport`
Windows:
goHornbillDocumentImport.exe -instanceid=yourinstanceid -apikey=yourapikey -csvd=docs_main.csv -csvc=docs_collections.csv -csvs=docs_shares.csv -csvt=docs_tags.csv -dryrun=true

## Updates

The utility will self-update when it detects that a newer minor or patch version has been released on GitHub. All of the import tools use semantic versioning, where:

- Major version updates will contain new feature(s), and include breaking changes to the tool configuration
- Minor version updates will contain new feature(s) and do not include breaking changes to the tool configuration
- Patch version updates will contain bug fixes only

When there is a Major version update, the utility will not self-update but instead will output a message to the command line & log(s) stating that there is a newer version available to download from GitHub. In this instance, you should download the latest release from GitHub, and review the breaking configuration changes against your existing import configuration before implementation. You can also check the Hornbill Community Forum for real-time release updates.

## Requirements

### Host

The program that performs the import is fairly lightweight and doesn’t require much in the way of hardware to run. It can be run on virtualized or physical hardware running any version of Windows currently supported by Microsoft, but basic guidelines are as follows:

- Operating System - Microsoft Windows, 32 or 64-bit, current/LTS, desktop/server
- CPU - Intel-compatible, one or more cores
- RAM - 4GB minimum

### Network

The utility connects to the Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use it without the need to make any proxy or firewall configuration changes.

#### **HTTP Proxies**

If you use a proxy for your internet traffic, the HTTP_PROXY and HTTPS_PROXY environment variables will need to be set. These environment variables hold the hostname or IP address of your proxy server. The proxy environment variables can be set from a command line as so:

``` cmd
set HTTP_PROXY=HOST:PORT
set HTTPS_PROXY=HOST:PORT
```
Where “HOST” is the IP address or hostname of your Proxy Server and “PORT” is the specific port number. If you require a username and password to go through the proxy, the format for the setting is as follows:

``` cmd
set HTTP_PROXY=username:password@HOST:PORT
set HTTPS_PROXY=username:password@HOST:PORT
```

#### **URL White Listing**

Occasionally, on top of setting the HTTP_PROXY and HTTPS_PROXY variables, the following URLs may need to be white-listed to allow access out from your network to the required endpoints in the Hornbill network:

- `https://files.hornbill.com/instances/INSTANCENAME/zoneinfo` - Allows access to lookup your Instance API Endpoint
- `https://files.hornbill.co/instances/INSTANCENAME/zoneinfo` - Backup URL for when `files.hornbill.com` is unavailable
- `https://eurapi.hornbill.com/INSTANCENAME/xmlmc/` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
- `https://api.github.com/repos/hornbill/asset-rel-import/tags` - Allows the utility to self-update. **Optional**
