# Authorization

## API Key Rules

This utility uses ([API keys](https://docs.hornbill.com/esp-fundamentals/security/api-keys)):

- activity:postMessage
- bpm:processSpawn2
- session:getApplicationOption
- admin:groupGetInfo
- admin:userGetInfo
- data:entityAddRecord
- data:entityAttachFile
- data:entityBrowseRecords
- data:entityBrowseRecords2
- data:entityUpdateRecord
- data:profileCodeLookup
- data:queryExec
- mail:decodeCompositeMessage
- session:userLogoff
- session:userLogon
- system:logMessage
- apps/com.hornbill.servicemanager/RelationshipEntities:add
- apps/com.hornbill.servicemanager/Requests:holdRequest

## HTTP Proxies

If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
``set HTTP_PROXY=HOST:PORT``

``set HTTPS_PROXY=HOST:PORT``
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:
``set HTTP_PROXY=username:password@HOST:PORT``

``set HTTPS_PROXY=username:password@HOST:PORT``
### **URLs to White List**

Occasionally on top of setting the HTTP_PROXY variable the following URLs need to be white listed to allow access out to our network

- https://files.hornbill.com/instances/INSTANCENAME/zoneinfo - Allows access to lookup your Instance API Endpoint
- https://files.hornbill.co/instances/INSTANCENAME/zoneinfo - Backup URL for when files.hornbill.com is unavailable
- https://eurapi.hornbill.com/INSTANCENAME/xmlmc/ - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
- https://api.github.com/repos/hornbill/asset-rel-import/tags - Allows the utility to self-update. Optional