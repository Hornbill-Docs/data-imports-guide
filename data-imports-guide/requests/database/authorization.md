# Authorization

## Api Key Rules 

This utility uses ([API keys](https://docs.hornbill.com/esp-fundamentals/security/api-keys)):

- activity:postMessage
- bpm:processSpawn2
- session:getApplicationOption
- admin:userGetInfo
- data:entityAddRecord
- data:entityBrowseRecords
- data:entityBrowseRecords2
- data:entityUpdateRecord
- data:profileCodeLookup
- data:queryExec
- data:updateRecord
- apps/com.hornbill.servicemanager/RelationshipEntities:add
- apps/com.hornbill.servicemanager/Requests:holdRequest

## HTTP Proxie

If you use a proxy for all of your internet traffic, the HTTP_PROXY Environment variable needs to be set. The https_proxy environment variable holds the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:
``set HTTP_PROXY=HOST:PORT``
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number.