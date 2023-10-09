# Configuration

## Configuration Overview

A demonstration configuration file is provided within the package, which includes configuration for importing request data from your data source. If a configuration file is not specified as a command line argument when executing the tool, then a default configuration file named conf.json, containing the correct JSON, must exist. The following configuration file contains the configuration elements required when importing request data:

## Example Configuration File

```json
"HBConf": {
   "InstanceID": "",
   "APIKeys":[]
 },
 "AppDBConf": {
   "Driver": "swsql",
   "Server": "127.0.0.1",
   "Database": "",
   "UserName": "",
   "Password": "",
   "Port": 5002,
   "Encrypt": false
 },
 "CustomerType": 2,
 "ContactKeyColumn":"h_logon_id",
 "DateTimeFormat":"2006-01-02 15:04:05",
 "AllowUnpublishedCatalogItems": false,
 "LinkedRequestPostVilibility": "customer",
 "RequestTypesToImport": 
 [
   {
     "Description":"This object configures the importing of Incidents",
     "Import":true,
     "ServiceManagerRequestType": "Incident",
     "AppRequestType":"Incident",
     "DefaultTeam":"Service Desk",
     "DefaultOwner": "ownerid",
     "DefaultPriority":"Low",
     "DefaultService": "Communications",
     "DefaultCatalog": 3,
     "RequestQuery":"SELECT 'IN0000123' AS parentref, opencall.callref, opencall.h_formattedcallref, FROM_UNIXTIME(logdatex) as logdate, FROM_UNIXTIME(closedatex) as closedate, cust_id, cust_name, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND opencall.callclass = 'Incident'",
     "RequestReferenceColumn":"h_formattedcallref",
     "RequestGUID":"callref",
     "ParentRequestRefColumn": "parentref",
     "CoreFieldMapping": {
       "h_datelogged":"[logdatex]",
       "h_dateclosed":"[closedatex]",
       "h_summary":"[itsm_title]",
       "h_description":"Imported Incident Reference: [h_formattedcallref]\n\n[updatetxt]",
       "h_external_ref_number":"[h_formattedcallref]",
       "h_fk_user_id":"[cust_id]",
       "h_status":"[status]",
       "h_request_language":"en-GB",
       "h_impact":"[itsm_impact_level]",
       "h_urgency":"[itsm_urgency_level]",
       "h_customer_type":"0",
       "h_container_id":"",
       "h_fk_serviceid":"",
       "h_resolution":"",
       "h_category_id":"[probcode]",
       "h_closure_category_id":"[fixcode]",
       "h_ownerid":"[owner]",
       "h_fk_team_id":"[suppgroup]",
       "h_fk_priorityid":"",
       "h_site_id":"[site]",
       "h_company_id":"",
       "h_company_name":"",
       "h_withinfix":"[withinfix]",
       "h_withinresponse":"[withinresp]",
       "h_custom_a":"",
       "h_custom_b":"",
       "h_custom_c":"",
       "h_custom_d":"",
       "h_custom_e":"",
       "h_custom_f":"",
       "h_custom_g":"",
       "h_custom_h":"",
       "h_custom_i":"",
       "h_custom_j":"",
       "h_custom_k":"",
       "h_custom_l":"",
       "h_custom_m":"",
       "h_custom_n":"",
       "h_custom_o":"",
       "h_custom_p":"",
       "h_custom_q":""
     },
     "AdditionalFieldMapping":{
       "h_firsttimefix":"",
       "h_custom_a":"Custom Data",
       "h_custom_b":"[itsm_title]",
       "h_custom_c":"[updatetxt]",
       "h_custom_d":"",
       "h_custom_e":"",
       "h_custom_f":"",
       "h_custom_g":"",
       "h_custom_h":"",
       "h_custom_i":"",
       "h_custom_j":"",
       "h_custom_k":"",
       "h_custom_l":"",
       "h_custom_m":"",
       "h_custom_n":"",
       "h_custom_o":"",
       "h_custom_p":"",
       "h_custom_q":"",
       "h_flgproblemfix":"",
       "h_fk_problemfixid":"",
       "h_flgfixisworkaround":"",
       "h_flg_fixisresolution":""
     },
     "RequestHistoricUpdateQuery":"SELECT *, FROM_UNIXTIME(updatetime) AS udtime FROM updatedb WHERE callref = {RequestGUID}",
     "HistoricUpdateMapping":{
       "h_updatedate":"[udtime]",
       "h_timespent":"[timespent]",
       "h_updatetype":"[udtype]",
       "h_updateindex":"[udid]",
       "h_updateby":"[aaid]",
       "h_updatebygroup":"[groupid]",
       "h_actiontype":"[udtype]",
       "h_actionsource":"[udsource]",
       "h_description":"[updatetxt]"
     }
   },
   {
     "Description":"This object configures the importing of Service Requests",
     "Import":true,
     "ServiceManagerRequestType": "Service Request",
     "AppRequestType":"Service Request",
     "DefaultTeam":"Service Desk",
     "DefaultOwner": "ownerid",
     "DefaultPriority":"Low",
     "DefaultService": "Communications",
     "DefaultCatalog": 5,
     "RequestQuery":"SELECT opencall.callref, opencall.h_formattedcallref, FROM_UNIXTIME(logdatex) as logdate, FROM_UNIXTIME(closedatex) as closedate, cust_id, cust_name, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND opencall.callclass = 'Service Request'",
     "RequestReferenceColumn":"h_formattedcallref",
     "RequestGUID":"callref",
     "CoreFieldMapping": {
       "h_datelogged":"[logdatex]",
       "h_dateclosed":"[closedatex]",
       "h_summary":"[itsm_title]",
       "h_description":"Imported Incident Reference: [h_formattedcallref]\n\n[updatetxt]",
       "h_external_ref_number":"[h_formattedcallref]",
       "h_fk_user_id":"[cust_id]",
       "h_status":"[status]",
       "h_request_language":"en-GB",
       "h_impact":"[itsm_impact_level]",
       "h_urgency":"[itsm_urgency_level]",
       "h_customer_type":"0",
       "h_container_id":"",
       "h_fk_serviceid":"",
       "h_resolution":"",
       "h_category_id":"[probcode]",
       "h_closure_category_id":"[fixcode]",
       "h_ownerid":"[owner]",
       "h_fk_team_id":"[suppgroup]",
       "h_fk_priorityid":"",
       "h_site_id":"[site]",
       "h_company_id":"",
       "h_company_name":"",
       "h_withinfix":"[withinfix]",
       "h_withinresponse":"[withinresp]",
       "h_custom_a":"",
       "h_custom_b":"",
       "h_custom_c":"",
       "h_custom_d":"",
       "h_custom_e":"",
       "h_custom_f":"",
       "h_custom_g":"",
       "h_custom_h":"",
       "h_custom_i":"",
       "h_custom_j":"",
       "h_custom_k":"",
       "h_custom_l":"",
       "h_custom_m":"",
       "h_custom_n":"",
       "h_custom_o":"",
       "h_custom_p":"",
       "h_custom_q":""
     },
     "AdditionalFieldMapping":{
       "h_custom_a":"",
       "h_custom_b":"",
       "h_custom_c":"",
       "h_custom_d":"",
       "h_custom_e":"",
       "h_custom_f":"",
       "h_custom_g":"",
       "h_custom_h":"",
       "h_custom_i":"",
       "h_custom_j":"",
       "h_custom_k":"",
       "h_custom_l":"",
       "h_custom_m":"",
       "h_custom_n":"",
       "h_custom_o":"",
       "h_custom_p":"",
       "h_custom_q":"",
       "h_flgproblemfix":"",
       "h_fk_problemfixid":"",
       "h_flgfixisworkaround":"",
       "h_flg_fixisresolution":""
     },
     "RequestHistoricUpdateQuery":"SELECT *, FROM_UNIXTIME(updatetime) AS udtime FROM updatedb WHERE callref = {RequestGUID}",
     "HistoricUpdateMapping":{
       "h_updatedate":"[udtime]",
       "h_timespent":"[timespent]",
       "h_updatetype":"[udtype]",
       "h_updateindex":"[udid]",
       "h_updateby":"[aaid]",
       "h_updatebygroup":"[groupid]",
       "h_actiontype":"[udtype]",
       "h_actionsource":"[udsource]",
       "h_description":"[updatetxt]"
     }
   }
 ],
 "PriorityMapping": {
   "External Priority":"Service Manager Priority"
 },
 "TeamMapping": {
   "External Group ID":"Service Manager Team Name"
 },
 "CategoryMapping": {
   "External Profile Code":"Service Manager Profile Code"
 },
 "ResolutionCategoryMapping": {
   "External Resolution Profile Code":"Service Manager Resolution Profile Code"
 },
 "ServiceMapping": {
   "External Service Name":"Service Manager Service Name"
 },
 "ServiceCatalogItemMapping": {
   "External Service Catalog Item Name": 1
 },
 "StatusMapping":{
   "1" : "status.open",
   "2" : "status.open",
   "3" : "status.open",
   "4" : "status.onHold",
   "5" : "status.open",
   "6" : "status.resolved",
   "8" : "status.new",
   "9" : "status.open",
   "10" : "status.open",
   "11" : "status.open",
   "16" : "status.closed",
   "17" : "status.cancelled",
   "18" : "status.closed"
 }
 ```

## What Do I Put In The Configuration File?

### **HBConfig**
Connection information for the Hornbill instance:

- **InstanceID** - This is the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.hornbill.com/**instance name**/. This value is case sensitive.
- **APIKeys** - This is an array of API Keys with which the tool will log the new requests. A minimum of 1 and a maximum of 10 can be defined. The more API keys provided, the more workers will import requests concurrently. API Keys can be created against the same user account in Hornbill. Details on how to create an API key can be found [here](https://docs.hornbill.com/esp-fundamentals/security/api-keys).

### **AppDBConf**
Contains the connection information for the database (direct or via ODBC) that contains the request data for import.

- **Driver** - the driver to use to connect to the database that holds the request data:
    - **swsql** : Supportworks 7.x SQL (MySQL v4.0.16). Also supports MySQL v3.2.0 to <v5.0
    - **mysql** : MySQL Server v5.0 or above, or MariaDB
    - **mssql** : Microsoft SQL Server (2005 or above)
    - **odbc** : An ODBC connection that resides on the machine performing the import
    - **csv** : An ODBC connection that resides on the machine performing the import, which is specifically using the "Microsoft Access Text Driver", and is configured to look at a folder containing CSV files.
- **Server** - The address for the database host (or 127.0.0.1 if using odbc or csv drivers)
- **UserName** - The username for the SQL database
**Password** - Password for above User Name
**Port** - SQL port if connecting to a database directly
**Encrypt** - Boolean value to specify whether the connection between the script and the database should be encrypted. NOTE: There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to true if your SQL Server has been patched accordingly.

### **CustomerType**
Should contain an integer value of 0 or 1, to determine the customer type for the records being imported:

- 0 - Hornbill Users
- 1 - Hornbill Contacts
- 2 - This will first search through the Users for a matching account, and if none is found then will search through the Contacts for a matching customer

### **ContactKeyColumn**
When searching through active Contacts for customer records (selecting either option 1 or 2 above), this needs populating with the unique column to search against. Examples:

- h_logon_id - this is the login ID column for the contact
- h_email_1 - this is the primary email address of the contact

### **DateTimeFormat**

When this parameter is populated, a parser in the tool will take the mapped datetime values from the request queries (h_datelogged, h_dateresolved and h_dateclosed from CoreFieldMapping, and h_updatedate from HistoricUpdateMapping), along with the value of this parameter, and convert the datetime into a Hornbill-compatible datetime string. See the format constants in the [Go time format documentation] for more information on populating this field.

### **AllowUnpublishedCatalogItems**
When set to ``true``, the tool will ignore the Status of the Service Catalog Items for requests being imported, allowing requests to be imported with Catalog Items in a Draft or Retired status. When set to ``false``, if the target Catalog Item is Draft or Retired, then the Catalog Item will not be associated to the request.

### **LinkedRequestPostVilibility**
Defines the visibility of the Timeline updates applied when imported requests and linked with parent requests. Can be set to customer or team. See ParentRequestRefColumn in RequestTypesToImport.

### **RequestTypesToImport**
A JSON array of objects that contain request-type specific configuration.

- **Description** - a string that allows you to describe the current request type object within the array. This is not used by the tool, so can contain any text string.
- **Import** - boolean true/false. Specifies whether the current class section should be included in the import.
- ServiceManagerRequestType - specifies the Service Manager request type that the current Conf section relates to.
- **AppRequestType** - specifies the request type of the requests being imported.
- **DefaultTeam** - If a request is being imported, and the tool cannot verify its Support Group, then the Support Group from this variable is used to assign the request.
- **DefaultOwner** - If a request is being imported, and the tool cannot verify its Owner, then the Analyst ID from this variable is used to assign the request.
- **DefaultPriority** - If a request is being imported, and the tool cannot verify its Priority, then the Priority from this variable is used to escalate the request.
- **DefaultService** - If a request is being imported, and the tool cannot verify its Service from the mapping, then the Service from this variable is used to log the request.
- **DefaultCatalog** - If the DefaultService (above) is being used for a record, then populating this with the primary key of the Catalog Item set against the Service for the Request Type being imported will ensure that the Catalog Item, and associated BPM Workflow, will be used when creating the Request. NOTE - the Catalog Item Primary Key value must be for the default language record, and not a translated catalog item record.
- **RequestQuery** - The query used to retrieve request data from your data source
    - When using a SQL source, this would be a SQL query
    - When using an ODBC source, this query would be specific to that ODBC connector, for example:
    - When the ODBC is using CSV files as its data source, the RequestQuery string may look like: SELECT * FROM requestImport.csv
    - When the ODBC is using The Microsoft Excel driver, and therefore an XLS or XLSX files as its data source, the RequestQuery string may look like the following, where [requestData$] refers to the worksheet in the spreadsheet that contains the request data: SELECT * FROM [requestData$]
    - IMPORTANT NOTE - When using an ODBC source to connect to CSV or Excel spreadsheets using the Microsoft Excel ODBC Driver, the SQL Query will not properly support DISTINCT or UNION statements and if using these then your data may be truncated to 255 characters per column. This is a limitation in the Microsoft Excel ODBC Driver and not this tool. The tool has extra code to help with this limitation and prevent duplication of records.
- **RequestReferenceColumn** - This defines the column returned by the above query that holds the friendly name of the request reference
- **RequestGUID** - This defines the column returned by the above query that holds the ID of the request reference. Depending on your data source, this may be the same as RequestReferenceColumn. This is used when searching for import request specific historic update data.
- **ParentRequestRefColumn** - This defines the column returned by the above query that holds the ID of the Hornbill Request Reference, for the request that should be linked to the request being imported.
- **CoreFieldMapping** - The core fields used by the API calls to raise requests within Service Manager, and how the imported data should be mapped into these fields.
    - Any value wrapped with [] will be populated with the corresponding response from the request Query
    - Any Other Value is treated literally as the written example:
        - "h_summary":"[itsm_title]", - the value of itsm_title is taken from the SQL output and populated within this field
        - "h_description":"Incident Reference: [oldCallRef]\n\n[updatetxt]", - the request description would be populated with "Incident Reference: ", followed by the imported request reference, 2 new lines then the description text from the request.
- Any Hornbill Date Field being populated should have a datetime stamp passed to it, in the following format: YYYY-MM-DD HH:MM:SS
- Note: "h_dateclosed":"[closedatex]", - opencall.closedatex is used to hold the date a request will come off hold. This must be populated if you are importing requests from in an On-Hold or Closed status.
- Note: "h_custom_*" - if you're mapping data to any of the custom fields in the CoreFieldMapping, then you also MUST map the data into the corresponding custom field in the AdditionalFieldMapping.
- Core Fields that can resolve associated record from passed-through value:
    - "h_site_id":"[site]", - When a string is passed to the site field, the script attempts to resolve the given site name against the Site entity, and populates the request with the correct site information. If the site cannot be resolved, the site details are not populated for the request being imported.
    - "h_fk_user_id":"[cust_id]", - As site, above, but resolves the original request customer against the users or contacts within Hornbill.
    - "h_ownerid":"[owner]", - As site, above, but resolves the original request owner against the analysts within Hornbill.
    - "h_category_id":"[probcode]", - As site, above, but uses additional CategoryMapping from the configuration, as detailed below.
    - "h_closure_category_id":"[fixcode]", - As site, above, but uses additional ResolutionCategoryMapping from the configuration, as detailed below.
    - "h_ownerid":"[owner]", - As site, above, but resolves the original request owner against the analysts within Hornbill.
    - "h_fk_team_id":"[suppgroup]", - As site, above, but uses additional TeamMapping from the configuration, as detailed below.
    - "h_fk_priorityid":"[priority]", - As site, above, but uses additional PriorityMapping from the configuration, as detailed below.
    - "h_fk_service_id":[serviceid]" - As site, above, but uses additional ServiceMapping from the configuration, as detailed below.
    - "h_catalog_id":[serviceid]" - As site, above, but uses additional ServiceCatalogItemMapping from the configuration, as detailed below.
- AdditionalFieldMapping - Contains additional columns that can be stored against the new request record. Mapping rules are as above.
- PublishedMapping - Contains additional configuration to allow the publishing of Problem and Known Error records being imported:
    - Publish - Boolean, whether or not to publish the imported records
    - Description - Description of the published records
    - LanguageCode - Default language code of the published records
    - reateEnglish - Boolean, whether an English translation should be created, in the case of LanguageCode not being English
    - PublishedStatus - Can be publish or draft
    - DatePublished - The date the record was published
    - ShowWorkaround - Show the workaround against the published record
    - Workaround - The workaround content
    - LastUpdated - The datetime that the published record was last updated
- RequestHistoricUpdateQuery - The query used to retrieve request diary update data from your data source. To inject the request GUID as found in the rows returned by the RequestQuery, add {RequestGUID} in to the appropriate clause. For example, in a CSV import this may look like: SELECT * FROM diaryImport.csv WHERE CALLREFERENCE = '{RequestGUID}'. If this as an empty string, the tool will not try to add any historic update records.
- HistoricUpdateMapping - The fields used by the API calls to store historic update records within Service Manager, and how the imported data should be mapped in to these fields. See **CoreFieldMapping** above for more information on how these should be mapped.
- **PriorityMapping**
Allows for the mapping of Priorities between your source data and Hornbill Service Manager, where the left-side properties list the Priorities from the import source, and the right-side values are the corresponding Priorities from Hornbill that should be used when escalating the new requests.
- **TeamMapping**
Allows for the mapping of Support Groups/Team between your source data and Hornbill Service Manager, where the left-side properties list the Support Group ID's from your source data, and the right-side values are the corresponding Team names from Hornbill that should be used when assigning the new requests.

- **CategoryMapping**
Allows for the mapping of Problem Profiles/Request Categories between your source data and Hornbill Service Manager, where the left-side properties list the Profile Codes from your source data, and the right-side values are the corresponding Profile Codes from Hornbill that should be used when categorising the new requests.

- **OwnerMapping**
Allows for the mapping of Owner IDs between your source data and Hornbill Service Manager, where the left-side properties list the Owner IDs from your source data, and the right-side values are the corresponding User IDs from Hornbill that should be used when assigning the imported requests.

- **ResolutionCategoryMapping**
Allows for the mapping of Resolution Profiles/Resolution Categories between your source data and Hornbill Service Manager, where the left-side properties list the Resolution Codes from your source data, and the right-side values are the corresponding Resolution Codes from Hornbill that should be used when applying Resolution Categories to the newly logged requests.

- **ServiceMapping**
Allows for the mapping of Services between your source data and Hornbill Service Manager, where the left-side properties list the Service names from your source data, and the right-side values are the corresponding Services from Hornbill that should be used when raising the new requests.

- **ServiceCatalogItemMapping**
Allows for the mapping of Services between your source data and Hornbill Service Manager, where the left-side properties list the Service Catalog Item names from your source data, and the right-side values are the corresponding primary keys (integer values) for the default language Catalog Item records from Hornbill that should be used when raising the new requests.

- **StatusMapping**
Allows for the mapping of Request Statuses between your source data and Hornbill Service Manager, where the left-side properties list the Status IDs from your source data, and the right-side values are the corresponding Status IDs from Hornbill that should be used when importing the requests.