# Overview

## Introduction to the Hornbill Service Manager Import Utility

The utility provides a simple, safe and secure way to migrate call data from a database or ODBC connection in to Hornbill Service Manager. The tool is designed to run behind your corporate firewall, and requires access to your request data host(s) or ODBC connection.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

The following tasks are carried out when the tool is executed:

- Request data is extracted as per your specification, as outlined in the Configuration section of this document;
- New requests are raised on Service Manager using the extracted request data and associated mapping specifications;
- Request diary entries from your source data are imported as Historic Updates against the new Service Manager Requests;
- Business process workflows are spawned and associated to your new requests (where relevant).

## Open Source

The Hornbill Service Manager Request Import Utility is provided open source under the [Hornbill Community Licence](https://wiki.hornbill.com/index.php/The_Hornbill_Community_License_(HCL)) and can be found [Here](https://github.com/hornbill/goHornbillRequestImport) on GitHub

## Installation Overview
### **Installation**

- Download the OS and architecture specific [ZIP archive](https://github.com/hornbill/goHornbillRequestImport/releases/latest) extract the zip file into a folder you would like the application to run from e.g. ``C:\request_import\`` - within the local user profile of the user who will run the tool. For example:
Windows: ``goHornbillRequestImport.exe -dryrun=true``

## HTTP Proxie

If you use a proxy for all of your internet traffic, the HTTP_PROXY Environment variable needs to be set. The https_proxy environment variable holds the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
```cmd
set HTTP_PROXY=HOST:PORT
```
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number.