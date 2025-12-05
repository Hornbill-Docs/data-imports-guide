# Simple List Import Utility
The utility provides a simple, safe and secure way to populate list data from a database or ODBC connection in to any of the Simple Lists within any of the applications within the Hornbill Collaboration Tool. The tool is designed to run behind your corporate firewall, and requires access to your request data host(s) or ODBC connection.

The tool connects to your Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use the tool without the need to make any firewall configuration changes.

The following tasks are carried out when the tool is executed:

* List data is extracted as per your specification, as outlined in the Configuration section of this document
* New list entries are created within the configured application's Simple List.
* The list value needs to be unique within the application's particular list.

## Installation Overview

### Windows Installation

* Download the OS and architecture-specific ZIP archive
* Extract zip into a folder you would like the application to run from e.g. **C:\\list\_import\\**
* Open **conf.json** and add in the necessary configuration
* Open a Command Line Prompt as Administrator
* Change Directory to the folder containing the utility **C:\\list\_import\\**
* Determine the appropriate executable and possibly rename it to remove confusion.
* Run the command relevant to the OS of the machine you are running this on:

Windows:`goHornbillSimpleListImport.exe -dryrun=true` 

## Configuration Overview

A demonstration configuration file is provided within the package, which includes configuration for importing request data from your data source. If a configuration file is not specified as a command line argument when executing the tool, then a default configuration file named conf.json, containing the correct JSON, must exist. The following configuration file contains the configuration elements required when importing request data:

### Example Configuration file
```json
{
  "HBConf": {
        "InstanceID": "yourInstanceId",
        "APIKeysKeysafeIDs": [
            11,12,13
        ]
    },
    "AppDBConf": {
        "keysafeKeyID": 2,
        "Driver": "swsql",
        "Encrypt": false
    },
    "Lists": [
        {
            "SQL": "SELECT val, disp FROM swlists",
            "Rebuild": false,
            "Application": "com.hornbill.servicemanager",
            "ListName": "someList",
            "ItemValue": "[val]",
            "DefaultDisplay": "[disp]",
            "Translations": [
                {
                    "Language": "en-GB",
                    "Display": "[disp]"
                }
            ]
        }
    ]
}
```
  
## What do I put in the Configuration file?

### HBConfig

Connection information for the Hornbill instance:

* **InstanceID** \- This is the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.hornbill.com/\[_**instance name**_\]/. This value is case sensitive.
* **APIKeys** \- This is an array of hornbill KeysafeIDs (of type API Key) with which the tool will log the new requests. A minimum of 1 and a maximum of 10 can be defined. The more API keys provided, the more workers will import requests concurrently. API Keys can be created against the same user account in Hornbill

### AppDBConf

Contains the connection information for the database (direct or via ODBC) that contains the request data for import.

* **Driver** \- the driver to use to connect to the database that holds the request data:  
   * **swsql** : Supportworks 7.x SQL (MySQL v4.0.16). Also supports MySQL v3.2.0 to <v5.0  
   * **mysql** : MySQL Server v5.0 or above, or MariaDB  
   * **mssql** : Microsoft SQL Server (2005 or above)  
   * **odbc** : An ODBC connection that resides on the machine performing the import  
   * **csv** : An ODBC connection that resides on the machine performing the import, which is specifically using the "Microsoft Access Text Driver", and is configured to look at a folder containing CSV files.
* **Encrypt** \- Boolean value to specify whether the connection between the script and the database should be encrypted. _NOTE_: There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to true if your SQL Server has been patched accordingly.
* **KeysafeID** \- This is the ID of the keysafe entry for "database authentication"

#### Database Authentication keysafe Type

* **Server** \- The address for the database host (or 127.0.0.1 if using odbc or csv drivers)
* **UserName** \- The username for the SQL database
* **Password** \- Password for above User Name
* **Port** \- SQL port if connecting to a database directly
* **Encrypt** \- Boolean value to specify whether the connection between the script and the database should be encrypted. _NOTE_: There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to true if your SQL Server has been patched accordingly.

### Lists

A JSON array of objects that contain particular list settings:

* **SQL** \- the SQL string to obtain the information from the configured data-source
* **Rebuild** \- a boolean - if set to **true** the indicated list will be REMOVED before inserting the new items
* **Application** \- the technical name of the application for which the Simple Lists items are to be added (for Service Manager, this application would be: com.hornbill.servicemanager)
* **ListName** \- the name of the Simple List within the application - the list will be created if it doesn't already exists
* **ItemValue** \- a value of the list item (this needs to be unique in the list; any additional items being imported with the same value will be ignored (i.e. one cannot (yet) use the import tool to update the display names of values)) and will likely be fed from the SQL, so use the square brackets to obtain the value of the field.
* **DefaultDisplay** \- the display of the values. The square brackets can be used to obtain the value of the field returned from the SQL query.
* **Translations** \- an JSON array to indicate translations for the value on display.  
   * **Language** \- the language code (this can be from an SQL result as well)  
   * **Display** \- the display of the values. The square brackets can be used to obtain the value of the field returned from the SQL query.

## Command Line Parameters

* file - Defaults to \`conf.json\` - Name of the Configuration file to load
* dryrun - Defaults to \`false\` - Set to True and the XMLMC for the raising of new requests will not be called, and instead the generated XML for each request will be dumped to the log file. This is to aid in debugging the initial connection information.

## API Key Rules

This utility uses (API keys):

* data:listAddItem
* data:listDelete
* admin:keysafeGetKey

  
## Preparing to run the tool

* Open **conf.json** and add in the necessary configration;
* Open Command Line Prompt as Administrator;
* Change Directory to the folder with simple-list-import executables 'C:\\simple-list-import  
   * On 32 bit Windows PCs: simple-list-import.exe (windows-386)  
   * On 64 bit Windows PCs: simple-list-import.exe (amd64)
* Follow all on-screen prompts, taking careful note of all prompts and messages provided.

  
## On First Run

You will be prompted for two key pieces of information:

* **APIKey** \- A valid API key created against a Hornbill user account with enough rights to run and retrieve the report (case sensitive). Details on how to create an API key can be found **here**.
* **InstanceId** \- The Instance ID (also referred to as the instance name) can be found in the URL used by your organization to access your Hornbill instance i.e. `https://live.hornbill.com/**instanceid**/` (case sensitive).

This will create an export.cfg file, this file is encrypted and contains your instance ID and API Key. The user who first ran the tool will be the only one able to run the tool until this file is deleted.

  
## Troubleshooting

### HTTP Proxies

If you use a proxy for all of your internet traffic, the HTTP\_PROXY Environment variable needs to be set. The https\_proxy environment variable holds the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:  
`set HTTP_PROXY=HOST:PORT set HTTPS_PROXY=HOST:PORT`   
Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number.

### Testing Overview

If you run the application with the argument -dryrun=true then no requests will be raised within Service Manager - the XML used to raise requests will be saved in the log file so you can ensure the database mappings are correct before running the import.

`goHornbillSimpleListImport.exe -dryrun=true -file=conf.json` 

### Logging Overview

All logging output is saved in the log directory, in the same directory as the executable. The file name contains the date and time the import was run _**list\_import\_2015-11-06T14-26-13Z.log**_ 

### Common Error Messages

Below are some common errors that you may encounter in the log file and what they mean:

* _**\[ERROR\] Error Decoding Configuration File:.....**_  \- this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
* _**\[ERROR\] Database Query Error: read tcp 127.0.0.1:xxxx->127.0.0.1:xxxx: wsarecv: An established connection was aborted by the software in your host machine.**_  \- This is most likely due to an incorrect Username and/or password specified in the _AppDBConf_ section of the conf file. Check and confirm the username and password used to access your database.
* _**\[ERROR\] Database Query Error: driver: bad connection.**_  \- Like the error above, this can be associated with an incorrect Username and/or password specified in the _AppDBConf_ section of the conf file. Check and confirm the username and password used to access your database.

### Error Codes

* **100** \- Unable to create log File
* **101** \- Unable to create log folder
* **102** \- Unable to Load Configuration File

## Change Log

Click "Read More" to view the Change Log.

### v2.0.0 - 2025

Initial Release

