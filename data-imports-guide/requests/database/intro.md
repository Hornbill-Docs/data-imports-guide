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

- Download the OS nd architecture specific [ZIP archive](https://github.com/hornbill/goHornbillRequestImport/releases/latest)
- Extract zip into a folder you would like the application to run from e.g. C:\request_import\
- Open **conf.json** and add in the necessary configuration
- Open a Command Line Prompt as Administrator
- Change Directory to the folder containing the utility C:\request_import\
- Run the command relevant to the OS of the machine you are running this on:
Windows: ``goHornbillRequestImport.exe -dryrun=true``

OSX: ``./goHornbillRequestImport -dryrun=true``