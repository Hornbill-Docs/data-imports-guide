# Configuration

## Configuration Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

A demonstration configuration file is provided within the package, which includes configuration for importing call data from Supportworks 7.6.x running ITSM Enterprise 3.6.x. This can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument
:::

### Example Configuration File

Click "Read More" to expand this section and show an example conf.json file
```json
{
  "HBConf": {
    "InstanceID": "",
    "UserName": "",
    "Password": ""
  },
  "SWServerAddress":"127.0.0.1",
  "AttachmentRoot":"E:/Program Files/Hornbill/Supportworks Server/data/cfa_store",
  "SWSystemDBConf": {
    "Driver":"swsql",
    "UserName": "",
    "Password": ""
  },
  "SWAppDBConf": {
    "Driver": "swsql",
    "Server": "127.0.0.1",
    "Database": "swdata",
    "UserName": "",
    "Password": "",
    "Port": 5002,
    "Encrypt": false
  },
  "CustomerType": "0",
  "SMProfileCodeSeperator": "-",
  "RelatedRequestQuery":"(SELECT fk_callref_m AS parentRequest, fk_callref_s AS childRequest from cmn_rel_opencall_oc) UNION (SELECT bpm_parentcallref AS parentRequest, callref AS childRequest FROM opencall WHERE callclass = 'B.P Task') ",
  "CallDiaryQuery": "SELECT updatetimex, repid, groupid, udsource, udcode, udtype, updatetxt, udindex, timespent FROM updatedb WHERE callref = [sourceref]",
  "RequestTypesToImport": 
  [
    {
      "Description":"This object configures the importing of Incidents",
      "Import":true,
      "CallClass": "Incident",
      "SupportworksCallClass":"Incident",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Communications",
      "SQLStatement":"SELECT opencall.callref,  logdatex, closedatex, cust_id, cust_name, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Incident' AND appcode = 'ITSM'",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Incident Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
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
        "h_site":"[site]",
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
      }
    },
    {
      "Description":"This object configures the importing of Workflow Tasks",
      "Import":true,
      "CallClass": "Incident",
      "SupportworksCallClass":"B.P Task",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Communications",
      "SQLStatement":"SELECT opencall.callref,  logdatex, closedatex, cust_id, cust_name, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'B.P Task' AND appcode = 'ITSM'",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Task Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
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
        "h_site":"[site]",
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
      }
    },
    {
      "Description":"This object configures the importing of Service Requests",
      "Import":true,
      "CallClass": "Service Request",
      "SupportworksCallClass":"Service Request",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Desktop Support",
      "SQLStatement":"SELECT opencall.callref,  cust_id, logdatex, closedatex, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Service Request'  AND appcode = 'ITSM'",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Incident Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
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
        "h_site":"[site]",
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
        "h_custom_q":""
      }
    },
    {
      "Description":"This object configures the importing of Change Requests",
      "Import":false,
      "CallClass": "Change Request",
      "SupportworksCallClass":"Change Request",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Finances",
      "SQLStatement":"SELECT opencall.callref, cust_id, logdatex, closedatex, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Change Request'  AND appcode = 'ITSM' ",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Change Request Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
        "h_fk_user_id":"[cust_id]",
        "h_fk_user_name":"[cust_name]",
        "h_status":"[status]",
        "h_request_language":"en-GB",
        "h_impact":"[itsm_impact_level]",
        "h_urgency":"[itsm_urgency_level]",
        "h_customer_type":"0",
        "h_container_id":"",
        "h_fk_serviceid":"[service_name]",
        "h_resolution":"",
        "h_category_id":"[probcode]",
        "h_closure_category_id":"[fixcode]",
        "h_ownerid":"[owner]",
        "h_fk_team_id":"[suppgroup]",
        "h_fk_priorityid":"",
        "h_site":"[site]",
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
        "h_start_time":"",
        "h_end_time":"",
        "h_change_type":"",
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
        "h_scheduled":""
      }
    },
    {
      "Description":"This object configures the importing of Problems",
      "Import":false,
      "CallClass": "Problem",
      "SupportworksCallClass":"Problem",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Home Working",
      "SQLStatement":"SELECT opencall.callref, cust_id, logdatex, closedatex, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Problem'  AND appcode = 'ITSM' ",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Problem Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
        "h_fk_user_id":"[cust_id]",
        "h_fk_user_name":"[cust_name]",
        "h_status":"[status]",
        "h_request_language":"en-GB",
        "h_impact":"[itsm_impact_level]",
        "h_urgency":"[itsm_urgency_level]",
        "h_customer_type":"0",
        "h_container_id":"",
        "h_fk_serviceid":"[service_name]",
        "h_resolution":"",
        "h_category_id":"[probcode]",
        "h_closure_category_id":"[fixcode]",
        "h_ownerid":"[owner]",
        "h_fk_team_id":"[suppgroup]",
        "h_fk_priorityid":"",
        "h_site":"[site]",
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
        "h_workaround":"",
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
      }
    },
    {
      "Description":"This object configures the importing of Known Errors",
      "Import":false,
      "CallClass": "Known Error",
      "SupportworksCallClass":"Known Error",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Infrastructure Support",
      "SQLStatement":"SELECT opencall.callref, cust_id, logdatex, closedatex, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Known Error'  AND appcode = 'ITSM' ",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Known Error Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
        "h_fk_user_id":"[cust_id]",
        "h_fk_user_name":"[cust_name]",
        "h_status":"[status]",
        "h_request_language":"en-GB",
        "h_impact":"[itsm_impact_level]",
        "h_urgency":"[itsm_urgency_level]",
        "h_customer_type":"0",
        "h_container_id":"",
        "h_fk_serviceid":"[service_name]",
        "h_resolution":"",
        "h_category_id":"[probcode]",
        "h_closure_category_id":"[fixcode]",
        "h_ownerid":"[owner]",
        "h_fk_team_id":"[suppgroup]",
        "h_fk_priorityid":"",
        "h_site":"[site]",
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
        "h_solution":"",
        "h_root_cause":"",
        "h_steps_to_resolve":"",
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
      }
    },
    {
      "Description":"This object configures the importing of Releases",
      "Import":false,
      "CallClass": "Release",
      "SupportworksCallClass":"Release Request",
      "DefaultTeam":"Service Desk",
      "DefaultPriority":"Low",
      "DefaultService":"Finances",
      "SQLStatement":"SELECT opencall.callref, cust_id, logdatex, closedatex, itsm_title, owner, suppgroup, status, updatedb.updatetxt, priority, itsm_impact_level, itsm_urgency_level, withinfix, withinresp, bpm_workflow_id, probcode, fixcode, site FROM opencall, updatedb WHERE updatedb.callref = opencall.callref AND updatedb.udindex = 0 AND callclass = 'Release Request'  AND appcode = 'ITSM' ",
      "CoreFieldMapping": {
        "h_datelogged":"[logdatex]",
        "h_dateclosed":"[closedatex]",
        "h_summary":"[itsm_title]",
        "h_description":"Supportworks Release Reference: [oldCallRef]\n\n[updatetxt]",
        "h_external_ref_number":"[oldCallRef]",
        "h_fk_user_id":"[cust_id]",
        "h_fk_user_name":"[cust_name]",
        "h_status":"[status]",
        "h_request_language":"en-GB",
        "h_impact":"[itsm_impact_level]",
        "h_urgency":"[itsm_urgency_level]",
        "h_customer_type":"0",
        "h_container_id":"",
        "h_fk_serviceid":"[service_name]",
        "h_resolution":"",
        "h_category_id":"[probcode]",
        "h_closure_category_id":"[fixcode]",
        "h_ownerid":"[owner]",
        "h_fk_team_id":"[suppgroup]",
        "h_fk_priorityid":"",
        "h_site":"[site]",
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
        "h_start_time":"",
        "h_end_time":"",
        "h_release_type":"",
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
        "h_scheduled":""
      }
    }
  ],
  "PriorityMapping": {
    "Supportworks Priority":"Service Manager Priority"
  },
  "TeamMapping": {
    "Supportworks Group ID":"Service Manager Team Name"
  },
  "CategoryMapping": {
    "Supportworks Profile Code":"Service Manager Profile Code"
  },
  "ResolutionCategoryMapping": {
    "Supportworks Resolution Profile Code":"Service Manager Resolution Profile Code"
  },
  "ServiceMapping": {
    "Supportworks Service Name":"Service Manager Service Name"
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
}
```

### Configuration Example
#### **Hornbill Instance Specific Configuration Properties**
Connection information for the Hornbill instance:

- "InstanceID" - This is the name of your Hornbill instance and can be found within the URL you use to navigate to it: live.hornbill.com/[instance name]/. This value is case sensitive.
- "UserName" - Instance User Name with which the tool will log the new requests
- "Password" - Instance Password for the above User

#### **SWServerAddress**
The address of the Supportworks Server. If this tool is to be run on the Supportworks Server, then this should be set to localhost.

#### **AttachmentRoot**
This is the location of the Supportworks Call File Attachment Store.

#### **SWSystemDBConf**
Contains the connection information for the Supportworks cache database (sw_systemdb).

- "Driver" the driver to use to connect to the sw_systemdb database:
- swsql = Supportworks 7.x SQL (MySQL v4.0.16). Also supports MySQL v3.2.0 to <v5.0
- mysql = MySQL Server v5.0 or above, or MariaDB (Supportworks v8+)
- "UserName" Username for a user that has read access to the SQL database from the location of the tool
- "Password" Password for above User Name

#### **SWAppDBConf**
Contains the connection information for the Supportworks application database (swdata).

- "Driver" the driver to use to connect to the database that holds the Supportworks application information:
- swsql = Supportworks 7.x SQL (MySQL v4.0.16). Also supports MySQL v3.2.0 to <v5.0
- mysql = MySQL Server v5.0 or above, or MariaDB (Supportworks v8+)
- mssql = Microsoft SQL Server (2005 or above)
- "Server" The address of the SQL server
- "UserName" The username for the SQL database
- "Password" Password for above User Name
- "Port" SQL port (5002 if the data is hosted on the Supportworks server)
- "Encrypt" Boolean value to specify whether the connection between the script and the database should be encrypted. NOTE: There is a bug in SQL Server 2008 and below that causes the connection to fail if the connection is encrypted. Only set this to true if your SQL Server has been patched accordingly.

#### **CustomerType**
Should contain an integer value of 0 or 1, to determine the customer type for the records being imported:

- 0 - Hornbill Users
- 1 - Hornbill Contacts

#### **SMProfileCodeSeperator**
A string, to specify the Profile Code separator character in use on your Service Manager instance. By default this is a :

#### **RelatedRequestQuery**
The SQL query to run to return request associations from Supportworks, to allow associated imported requests to be linked in Service Manager. The output of the SQL query needs to consist of the following columns:

- parentRequest : holds the Supportworks request reference of the parent request
- childRequest : holds the Supportworks request reference of the child request

#### **CallDiaryQuery**
The query to retrieve call diare entries, used when importing historic call update. The default value is below. where ``[sourceref]`` will be replaced by the Supportworks call reference number by the tool:

``SELECT updatetimex, repid, groupid, udsource, udcode, udtype, updatetxt, udindex, timespent FROM updatedb WHERE callref = [sourceref]``

#### **RequestTypesToImport**
A JSON array of objects that contain request-type specific configuration.

- Description - a string that allows you to describe the current request type object within the array. This is not used by the tool, so can contain any text string.
- Import - boolean true/false. Specifies whether the current class section should be included in the import.
- CallClass - specifies the Service Manager request class that the current Conf section relates to.
- SupportworksCallClass - specifies the call class of the Supportworks requests being imported. Only used in the logging and display output of the tool, the actual Supportworks request class for each import should be defined in the query contained within the SQLStatement parameter.
- DefaultTeam - If a request is being imported, and the tool cannot verify its Support Group, then the Support Group from this variable is used to assign the request.
- DefaultPriority - If a request is being imported, and the tool cannot verify its Priority, then the Priority from this variable is used to escalate the request.
- DefaultService - If a request is being imported, and the tool cannot verify its Service from the mapping, then the Service from this variable is used to log the request.
- SQLStatement - The SQL query used to get call (and extended) information from the Supportworks application data.
- CoreFieldMapping - The core fields used by the API calls to raise requests within Service Manager, and how the Supportworks data should be mapped in to these fields.
    - Any value wrapped with [] will be populated with the corresponding response from the SQL Query
    - Any Other Value is treated literally as written example:
        - "h_summary":"[itsm_title]", - the value of itsm_title is taken from the SQL output and populated within this field
        - "h_description":"Supportworks Incident Reference: [oldCallRef]\n\n[updatetxt]", - the request description would be populated with "Supportworks Incident Reference: ", followed by the Supportworks call reference, 2 new lines then the call description text from the Supportworks call.
    - Any Hornbill Date Field being populated should have an EPOCH value passed to it. This includes h_datelogged, h_dateresolved and h_dateclosed.
    - Note: "h_dateclosed":"[closedatex]", - opencall.closedatex is used in Supportworks to hold the date a request will come off hold. This must be populated if you are importing requests in an On-Hold or Closed status.
    - Core Fields that can resolve associated record from passed-through value:
        - "h_site":"[site]", - When a string is passed to the site field, the script attempts to resolve the given site name against the Site entity, and populates the request with the correct site information. If the site cannot be resolved, the site details are not populated for the request being imported.
        - "h_fk_user_id":"[cust_id]", - As site, above, but resolves the original request customer against the users or contacts within Hornbill.
        - "h_ownerid":"[owner]", - As site, above, but resolves the original request owner against the analysts within Hornbill.
        - "h_category_id":"[probcode]", - As site, above, but uses additional CategoryMapping from the configuration, as detailed below.
        - "h_closure_category_id":"[fixcode]", - As site, above, but uses additional ResolutionCategoryMapping from the configuration, as detailed below.
        - "h_ownerid":"[owner]", - As site, above, but resolves the original request owner against the analysts within Hornbill.
        - "h_fk_team_id":"[suppgroup]", - As site, above, but uses additional TeamMapping from the configuration, as detailed below.
        - "h_fk_priorityid":"[priority]", - As site, above, but uses additional PriorityMapping from the configuration, as detailed below.
- **AdditionalFieldMapping** - Contains additional columns that can be stored against the new request record. Mapping rules are as above.

#### **PriorityMapping**
Allows for the mapping of Priorities between Supportworks and Hornbill Service Manager, where the left-side properties list the Priorities from Supportworks, and the right-side values are the corresponding Priorities from Hornbill that should be used when escalating the new requests.

The following query will be helpful when listing the Supportworks Priorities that exist and therefore need to be mapped in the configuration: ``SELECT DISTINCT priority FROM opencall``

#### **TeamMapping**
Allows for the mapping of Support Groups/Team between Supportworks and Hornbill Service Manager, where the left-side properties list the Support Group ID's (not the Group Name!) from Supportworks, and the right-side values are the corresponding Team names from Hornbill that should be used when assigning the new requests.

The following query will be helpful when listing the Supportworks Teams that exist and therefore need to be mapped in the configuration: ``SELECT DISTINCT suppgroup FROM opencall``

#### **CategoryMapping**
Allows for the mapping of Problem Profiles/Request Categories between Supportworks and Hornbill Service Manager, where the left-side properties list the Profile Codes (not the descriptions!) from Supportworks, and the right-side values are the corresponding Profile Codes (again, not the descriptions!) from Hornbill that should be used when categorizing the new requests.

The following query will be helpful when listing all the Supportworks Profile Codes (Logging Categories) that have ever been used against Supportworks calls: ``SELECT DISTINCT probcode from opencall``

This can be compared with the contents of the "probcode" table which includes the corresponding descriptions. The descriptions will help you understand what the codes relate to and therefore create suitable mappings: ``SELECT * FROM probcode``

#### **ResolutionCategoryMapping**
Allows for the mapping of Resolution Profiles/Resolution Categories between Supportworks and Hornbill Service Manager, where the left-side properties list the Resolution Codes (not the descriptions!) from Supportworks, and the right-side values are the corresponding Resolution Codes (again, not the descriptions!) from Hornbill that should be used when applying Resolution Categories to the newly logged requests.

The following query will be helpful when listing all the Supportworks Resolution Codes (Resolution Categories) that have ever been used against Supportworks calls: ``SELECT DISTINCT fixcode from opencall``

This can be compared with the contents of the "fixcode" table which includes the corresponding descriptions. The descriptions will help you understand what the codes relate to and therefore create suitable mappings: ``SELECT * FROM fixcode``

#### **ServiceMapping**
Allows for the mapping of Services between Supportworks and Hornbill Service Manager, where the left-side properties list the Service names from Supportworks, and the right-side values are the corresponding Services from Hornbill that should be used when raising the new requests.

The following query will be helpful when listing the Supportworks Services that exist and therefore need to be mapped in the configuration: ``SELECT DISTINCT sc_folio.service_name from opencall JOIN sc_folio ON sc_folio.fk_cmdb_id = opencall.itsm_fk_service``

Not all versions of Supportworks include the concept of services. Even in versions that do, it's possible for a call in Supportworks to exist without an associated service. In this scenario it's typical to map all calls in Supportworks without a service to a Hornbill Service such as "Imported Requests" or another preferred service.

#### **StatusMapping**
Allows for the mapping of Request Statuses between Supportworks and Hornbill Service Manager, where the left-side properties list the Status IDs from Supportworks, and the right-side values are the corresponding Status IDs from Hornbill that should be used when importing the requests.

The configuration file comes pre-populated with suggested mappings between the Supportworks and Service Manager statuses.

