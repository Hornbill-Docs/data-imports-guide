# Organizations

## Introduction

If you deliver a service to organisations external to your own, you will store them in Hornbill as Organisation records. They work in conjunction with contact records which can be associated to Organisation records. Organisation records can also be used to represent suppliers, or third parties. They operate externally to Hornbill so can’t collaborate but can request a service from you as a customer. Customer Manager allows organisations to be better managed so that you can improve the relationship you have with your customers.

## Features

### Organisation List
#### Add an Organisation

- Organisations can be added using the + **Create New** button
Complete the mandatory fields, and any optional fields. Select Create to add the organisation.

#### Organisations List

- You can configure the available display columns in the list to show the information that is important to you using the Cog Icon
- Click on a specific organisation record to view more information about that organisation
- Click on the **File Box** icon next to any organisation you wish to archive

#### Searching Organisations

- **Quick Filter** - Use to easily find a specific organisation record, searching against their Name
- **Advanced Filter** - Select the Funnel icon to expose additional search filters, including Industry, City, Country.
- **Archived** - Use the File Box icon to filter the list to organisations which have been archived
- **Export** - Use the Download Arrow icon to export the filtered list of organisations
- **Contacts** - View Contacts for the listed organisations
**Global Search** - with the filter switched to **Organisations** you can search for organisations by name, and see matching results displayed in the Organisation list

### Organisation Record
#### Manage Organisations

From an Organisations record the following options are available to you

- **Archive** - If an Organisation is no longer needed, mark them as Archived. This will remove them from general Organisation searches, but will still be accessible from the Organisations list and Archived Organisations's option.
- **Contacts** - View any existing organisation's contact's and add new ones
- **Logo** - Upload an organisations logo
- **Industry** - The industry field uses a Simple List data. The list should be called Industry in the Collaboration app.
- **Region** - The region field is hidden by default and it uses a Simple List data. The list should be called ExternalOrganisationRegions in the Collaboration app.

### Plug-ins
#### Customer Manager

If Customer Manager is also installed on your instance the following additional functionality will be available against each organisation

- **Activity Stream** - Collaborate, Post and Comment on topics relating to the organisation
- **Activities** - Create and manage activities related to the organisation, such as reviews
- **Members** - Link internal staff to the organisation and define their roles (Account Manager etc)
- **Documents** - Search for an link documents held in Document Manager directly to the organisation
- **Attachments** - Upload and hold attachments against the organisation (contracts etc)
- **Service Contracts (BETA)** - Define service contracts, including supported items for the organisation

### Service Manager

If Service Manager is also installed on your instance you will see a list of requests which have been logged against each organisation when viewing the organisations details

- Manage which of the organisations contacts who have portal access, can also cancel service requests, and view requests raised from other contacts at the organisation via the customer portal

### Administration

- Users with the **Form Designer** role, will be able to add custom fields, hide existing fields, edit their properties, and edit which sections of the organisation record, fields are displayed in. This can achieved by dragging and dropping the fields into the required sections. With the **Form Designer** role, users will see a Design option to go into edit mode.
- **Custom Buttons** - Create custom buttons on the organisations details. Typical use of organisations custom buttons could be to link to a document (held in Document Manager, or other externally accessible cloud solution)
    - To create custom button, define the required URL, add in the variable (if required), in the above example possibly a document id held in one of organisations custom fields, give it a title, tooltip, icon and colour