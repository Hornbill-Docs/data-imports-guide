# Introduction

## Overview

This tool provides functionality to allow the import of Task/Request data from ServiceNow in to Hornbill Service Manager.

The following tasks are carried out when the tool is executed:

- ServiceNow task data is extracted as per your specification, as outlined in the Configuration section of this document;
- New requests are raised on Service Manager using the extracted task data and associated mapping specifications;
- ServiceNow Task diary entries are imported as Historic Updates against the new Service Manager Requests;
- ServiceNow Task attachments are attached to the relevant imported Service Manager request;
- Previous parent-child relationships between ServiceNow Tasks are re-created by linking the newly-imported - Service Manager requests with their previous parents/children;
- ServiceNow Approval Tasks are imported as Hornbill Activity Tasks, and associated with the relevant imported Requests.

## Requirements
### Network
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
- ``https://files.hornbill.co/instances/INSTANCENAME/zoneinfo`` - Backup URL for when ``files.hornbill.com`` is unavailable
- ``https://eurapi.hornbill.com/INSTANCENAME/xmlmc/`` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
- ``https://api.github.com/repos/hornbill/asset-rel-import/tags`` - Allows the utility to self-update. Optional
