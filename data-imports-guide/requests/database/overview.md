# Introduction to the Hornbill Service Manager Import Utility

## Overview

The utility provides a simple, safe and secure way to migrate call data from a database, an ODBC connection, or CSV files in to Hornbill Service Manager. The tool is designed to run behind your corporate firewall, and requires access to your request data host(s), ODBC connection, or CSV files.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

## Installation

Download the OS and architecture specific [ZIP archive](https://github.com/hornbill/goHornbillRequestImport/releases/latest) extract the zip file into a folder you would like the application to run from e.g. ``C:\request_import\`` - within the local user profile of the user who will run the tool. For example:
Windows: 
``goHornbillRequestImport.exe -dryrun=true``

## Requirements
### Host
The utility connects to the Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use it without the need to make any proxy or firewall configuration changes.

#### **HTTP Proxies**

If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
```cmd
set HTTP_PROXY=HOST:PORT
set HTTPS_PROXY=HOST:PORT
```
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:
```cmd
set HTTP_PROXY=username:password@HOST:PORT
set HTTPS_PROXY=username:password@HOST:PORT
```
#### **URL White Listing**

Occasionally on top of setting the HTTP_PROXY variable the following URLs need to be white listed to allow access out to our network

- ``https://files.hornbill.com/instances/INSTANCENAME/zoneinfo`` - Allows access to lookup your Instance API Endpoint
- ``https://files.hornbill.co/instances/INSTANCENAME/zoneinfo`` - Backup URL for when files.hornbill.com is unavailable
- ``https://eurapi.hornbill.com/INSTANCENAME/xmlmc/`` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
- ``https://api.github.com/repos/hornbill/asset-rel-import/tags`` - Allows the utility to self-update. Optional
