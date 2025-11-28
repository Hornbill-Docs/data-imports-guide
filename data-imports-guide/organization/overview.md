# Organization Import

## About the Organization Import Utility
The utility provides a quick and easy method of uploading and updating organizations from an external data source into Hornbill instance.

## Open Source
The Hornbill SQL Organzsation Import Utility is provided open source under the Hornbill Community License and can be found Here on GitHub

## Installation Overview
### Windows Installation
* Download the ZIP archive relevant to your OS and architecture. This contains the tool executable file, configuration file and license;
* Extract the ZIP archive into a folder you would like the application to run from e.g. 'C:\organization_import\'.

## Configuration Overview
The configuration of this utility is managed through a JSON file (conf.json), which is supplied with each release:

```json
{   "APIKey": "your_api_key_here",
   "InstanceId": "your_instance_id_here",
   "OrganisationAction": "Create/Update/Both",
   "SQLConf": {
       "Driver": "",
       "Server": "",
       "Database": "",
       "UserName": "",
       "Password": "",
       "Port": 0,
       "Encrypt": false,
       "OrganizationName": "FIELD 1",
       "Query": "SELECT <field_list> FROM <table_or_file>"
   },
   "OrganizationMapping":{
       "organization_name":"FIELD 1",
       "address":"FIELD 2",
       "city":"FIELD 3",
       "state":"FIELD 4",
       "postal_code":"FIELD 5",
       "country":"FIELD 6",
       "industry":"FIELD 7",
       "phone_number":"FIELD 8",
       "website":"FIELD 9",
       "language":"FIELD 10",
       "custom_1":"FIELD 11",
       "custom_2":"FIELD 12",
       "custom_3":"FIELD 13",
       "custom_4":"FIELD 14",
       "custom_5":"FIELD 15",
       "custom_6":"FIELD 16",
       "custom_7":"FIELD 17",
       "custom_8":"FIELD 18",
       "custom_9":"FIELD 19",
       "custom_10":"FIELD 20",
       "custom_11":"FIELD 21"
   }
}
```

* **APIKey**. A Hornbill API key for a user account with the correct permissions to carry out all of the required API calls
* **InstanceId**. Hornbil instance ID. You can get the instance ID from your URL used to access Hornbill: https://live.hornbill.com/your_instance_id/. This value is case sensitive.
* **OrganisationAction**. One of the following values:
    * ***Create***. Import will only create new organizations in your instance. If the imported organization already exists and this is set to "Create" then the organization will not be created.
    * ***Update***. Import will only update existing organizations in your instance. If the imported organization does not exist and this is set to "Update" then the organization will not be updated.
    * ***Both***. Import will create new organizations and update existing organizations in your instance. If the imported organization does not exist and this is set to "Both" then the organization will be created. If the imported organization does exist and this is set to "Both" then the organization will be updated.
* **SQLConf**
    * ***Driver***. The driver to use to connect to the database that holds the organization information. Needs to be one of the following:
        * mssql = Microsoft SQL Server (2005 or above)
        * mysql = MySQL Server 4.1+, MariaDB
        * mysql320 = MySQL Server v3.2.0 to v4.0
        * csv = ODBC Data Source using MS Access Text Driver (*.txt, *.csv)
        * excel = ODBC Data Source using MS Excel Driver (*.xls, *.xlsx, *.xlsm, *.xlsb)
    * ***Server***. Provide the address of the SQL server (e.g. localhost) when `Driver` is set to mssql, mysql or mysql320.
    * ***Database***. The name of the Database to connect to when `Driver` is set to mssql, mysql or mysql320. The name of the ODBC connection when `Driver` is set to csv or excel.
    * ***UserName***. Thee SQL authentication Username to connect to the Database (if any, e.g. root) when `Driver` is set to mssql, mysql or mysql320.
    * ***Password***. The password for the above username (if any) when `Driver` is set to mssql, mysql or mysql320.
    * ***Port***. The SQL port (e.g. 5002) when `Driver` is set to mssql, mysql or mysql320.
    * ***Encrypt***. Boolean value to specify whether the connection between the script and the database should be encrypted.   
    * ***OrganizationName***. Specify which field from your data source contains the organization name.
    * ***Query***. This should be the SQL query to retrieve the organization records (e.g. SELECT * FROM <datbase_table_or_file>)

    :::note
    There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set `Encrypt` to true if your SQL Server has been patched accordingly.
    :::

* **OrganizationMapping**. Maps data from your data source into the generic Hornbill organization record. Any value wrapped with "" will be populated with the corresponding response from the SQL Query and is treated literally as a written example.

## Set up ODBC Connector for CSV files
To create a 64 bit ODBC connector for CSV files, you need to have Microsoft Access Text Driver installed. This comes with the Microsoft Access Database Engine which can be found as an MS 2010 Redistributable or as an optional part of the Microsoft Office Suite.

#### Step 1
1. In Windows menu open ODBC Data Sources app as Administrator (in Windows 10 you can type in "ODBC" in the search box on the taskbar to quickly find the ODBC Data Sources app).
1. In ODBC Data Sources app navigate to System DSN tab.
1. Click on Add button to "Create a New Data Source". In the list of drivers locate Microsoft Access Text Driver (*.txt, *.csv). If this driver is not present in the list then you don't have Microsoft Access Database Engine installed. Please refer to the notes above to find out how to install it.
1. Click on Finish button to progress to the next step.

#### Step 2
1. Type in a name for your Data Source - this name will be used for the "Database" parameter in the JSON config file.
1. Set up the directory/folder where the CSV files are located:
    * When configuring the connector for the first time the Use Current Directory checkbox is selected by default. Unselect this checkbox to enable Select Directory button and click it. Navigate to the directory/folder where the CSV files are located. select the folder then click Ok button.
    * When editing an existing CSV connector the Use Current Directory checkbox should be unselected and Select Directory enabled. If the folder containing the CSV files is already displayed then there is no need to do anything in this step.
1. Click on Options button to expand the current window.
1. In the "Extensions List" select the *.csv option.
1. Click on Define Format button to progress to the next step.

#### Step 3
1. If the directory/folder has been configured correctly in the previous step you should see the CSV file(s) in the Tables list. Select the file you need to use for the import.
1. If you have column headings in your CSV file activate Column Name Header checkbox
amend Rows to Scan option depending on how many records you are importing. By default, this is set to 25, if you are importing more then it needs to be increased. It does not have to be an exact number but it needs to be sufficient to ensure all records from your file are retrieved.
1. Set Characters option to ANSI
1. On the left hand side there is a Columns list. By default, in the ODBC data source we are configuring the columns in the file are set to FIELD1, FIELD2, etc. You can leave them as they are but you need to ensure the columns in this list matches the columns in the CSV file or you can customize them. This does not affect the CSV file in any form, this is only for the ODBC Data Source configuration.
    :::note
    When having column headers in the CSV file, using the Guess button will attempt to read the column headers and customize the columns in the list accordingly. This is not a failproof action (hence the name of the button) therefore you need to ensure the columns are configured correctly if the Guess button is used
    :::
    * Data Type = should always be Char unless there is a need to be another type
    * Name = should be a valid name. Please note that on occasion invalid characters are introduced in the column name (e.g. ï»¿). You need to remove these characters and then click on Modify button to save the changes. Invalid characters in column name can cause the tool to fail to import the records.
1. Click on Ok buttons all the way back to System DSN tab to finish the configuration.

:::note
When importing from multiple files, repeat this step for all the files you need to import.
:::

If the configuration is complete then in the directory/folder selected during configuration there should now be a file named schema.ini. This file stores the configuration performed in the above steps. If this file is missing then the configuration was not completed successfully. This files stores the configuration for any file in the respective directory /folder therefore when defining a format for another file, the configuration is appended into this file rather than creating a separate schema file.

## Execute
#### Command Line Parameters
* **file**. This should point to your JSON configuration file and by default looks for a file in the current working directory called conf.json. If this is present you don't need to have the parameter.
* **dryrun**. If set to True the XMLMC for Create and Update organizations will not be called and instead the XML will be dumped to the log file, so you can ensure the data mappings are correct before running the import. Defaults to `false`.

Example: `goHornbillOrgImport_x64.exe -file=conf.json`

## Preparing to run the tool
1. Open conf.json and add in the necessary configuration;
1. Open Command Line Prompt as Administrator;
1. Change directory to the folder with goHornbillOrgImport_x64 *.* executables 'C:\organization_import
    * On 32 bit Windows PCs: goHornbilOrgImport_x86.exe
    * On 64 bit Windows PCs: goHornbillOrgImport_x64.exe
1. Follow all on-screen prompts, taking careful note of all prompts and messages provided.


## API Key Rules
This utility uses (API keys):
* data:entityAddRecord
* data:entityBrowseRecords2
* data:entityUpdateRecord
* system:logMessage

## Troubleshooting
### Logging Overview
All Logging output is saved in the log directory in the same directory as the executable the file name contains the date and time the import was run 

Example log file name: `SQL_Organization_Import_YYYYMMDDHHMMSS.log`

### Common Error Messages
Below are some common errors that you may encounter in the log file and what they mean:

[ERROR] Error Decoding Configuration File:..... - this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.

#### Error Codes
* 100 - Unable to create log File
* 101 - Unable to create log folder
* 102 - Unable to Load Configuration File


## HTTP Proxies
If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For windows machines, it can be set from the command line using the following:

`set HTTP_PROXY=HOST:PORT`
`set HTTPS_PROXY=HOST:PORT`

Where "HOST" is the IP address or host name of your Proxy Server and "PORT" is the specific port number. IF you require a username and password to go through the proxy, the format for the setting is as follows:

`set HTTP_PROXY=username:password@HOST:PORT`
`set HTTPS_PROXY=username:password@HOST:PORT`

#### URLs to White List
Occasionally on top of setting the HTTP_PROXY variable the following URLs need to be white listed to allow access out to our network

* `https://files.hornbill.com/instances/INSTANCENAME/zoneinfo` - Allows access to lookup your Instance API Endpoint
* `https://files.hornbill.co/instances/INSTANCENAME/zoneinfo` - Backup URL for when files.hornbill.com is unavailable
* `https://eurapi.hornbill.com/INSTANCENAME/xmlmc/` - This is your Instance API Endpoint, eurapi can change so you should use the endpoint defined in the previous URL
* `https://api.github.com/repos/hornbill/goHornbillOrgImport/tags` - Allows the utility to self-update. Optional

## Scheduling Overview
#### Windows
You can schedule goHornbillOrgImport.exe to run with any optional command line argument from Windows Task Scheduler.

* Ensure the user account running the task has rights to goHornbillOrgImport.exe and the containing folder.
* Make sure the Start In parameter contains the folder where goHornbillOrgImport.exe resides in otherwise it will not be able to pick up the correct path.