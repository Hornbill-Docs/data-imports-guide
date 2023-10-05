# Overview

## About the Supportworks to Hornbill Service Manager Request Import Utility
The utility provides a simple, safe and secure way to migrate call data from Supportworks v7.x & v8.x in to Hornbill Service Manager. The tool is designed to run behind your corporate firewall, and requires access to your Supportworks database host(s) and file storage.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

The following tasks are carried out when the tool is executed:

- Supportworks call data is extracted as per your specification, as outlined in the Configuration section of this document;
- New requests are raised on Service Manager using the extracted call data and associated mapping specifications;
- Supportworks call diary entries are imported as Historic Updates against the new Service Manager Requests;
- Attachments to Supportworks Call Diary Entries are imported against their appropriate Historic Updates within Service Manager;
- Call attachments that are not related to Call Diary Entries are attached to the relevant Service Manager request;
- Call attachments of type SWM (Supportworks Mail) are decoded and stored as plain text attachments against the Service Manager request or Historic Update as appropriate.

### **Important Notice**
Importing Supportworks call data and associated file attachments into your Hornbill instance will consume your subscribed storage. Please check your Administration console and your Supportworks data prior to running this import to ensure that you have enough subscribed storage available.

When running the import tool, after the call records are imported, you will receive a warning before importing the associated call file attachments. Please take note of the information presented, as this will inform you the amount of Hornbill storage space you have available to your instance, and the approximate amount that will be consumed should you continue with the file attachment import.

## Open Source

The Supportworks to Hornbill Service Manager Request Import Utility is provided open-source under the [Hornbill Community Licence](https://wiki.hornbill.com/index.php/The_Hornbill_Community_License_(HCL)) and can be found [Here](https://github.com/hornbill/goSWRequestImport) on GitHub

## Installation Overview
### **Windows Installation**

- Download the [ZIP archive](https://github.com/hornbill/goSWRequestImport/releases/latest) relevant to your operating system and architecture
- Extract the zip into a folder you would like the application to run from e.g. C:\Call_Import\
- Open **conf.json** and add in the necessary configuration
- Open a Command Line Prompt as Administrator
- Change directory to the folder containing the utility C:\Call_Import\
- Run the command relevant to the architecture of the machine you are running this on:
``goSWRequestImport.exe -dryrun=true``