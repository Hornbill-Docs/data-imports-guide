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

## Installation Overview
### **Windows Installation**

Download the [ZIP archive](https://github.com/hornbill/goSWRequestImport/releases/latest) relevant to your operating system and architecture, extract the zip file into a folder within the local user profile of the user who will run the tool. Run the command relevant to the architecture of the machine you are running this on:
``goSWRequestImport.exe -dryrun=true``

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