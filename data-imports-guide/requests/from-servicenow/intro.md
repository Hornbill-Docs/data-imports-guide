# Overview

### Introduction

This tool provides functionality to allow the import of Task/Request data from ServiceNow in to Hornbill Service Manager.

The following tasks are carried out when the tool is executed:

- ServiceNow task data is extracted as per your specification, as outlined in the Configuration section of this document;
- New requests are raised on Service Manager using the extracted task data and associated mapping specifications;
- ServiceNow Task diary entries are imported as Historic Updates against the new Service Manager Requests;
- ServiceNow Task attachments are attached to the relevant imported Service Manager request;
- Previous parent-child relationships between ServiceNow Tasks are re-created by linking the newly-imported - Service Manager requests with their previous parents/children;
- ServiceNow Approval Tasks are imported as Hornbill Activity Tasks, and associated with the relevant imported Requests.
