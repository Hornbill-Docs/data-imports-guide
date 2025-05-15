# Importing Users from Google Workspace

## Overview

## Authentication

Before you can create an import configuration, you must first create a [Keysafe](/esp-fundamentals/security/keysafe) Key, which securely stores and manages the authentication credentials to give the Cloud Data Imports feature access to your Google Workspace user data.

The Keysafe Key Type to use is called **Google Workspace**, see the [Keysafe article](/esp-config/security/keysafe) in the Hornbill Platform Configuration Guide for setup instructions.

Once you have your Keysafe Key created and connected to your Google Workspace account, you can then move on to creating your first import.

## Creating your first import

### Finding Cloud Data Imports

To find the Cloud Data Imports area of your Hornbill instance, navigate to:

- Configuration
- Platform Configuration
- Data > Cloud Data Imports

### Create a New Import Configuration

- Click the `+ Add New` button in the top-right of the Cloud Data Imports view
- When prompted, give the Import Configuration a meaningful and unique name
- Click `Create`
- On the resulting form, optionally complete the **Description** field

### Choosing the Data Source

- Click on the `Data Source` tab
- In the `Data Source Settings` pane, click the edit button to the right hand side of the `Import` input
- In the window that appears, click on:
    - `Cloud Data Imports`
    - `Users`
    - `Google Workspace`
    - `Apply`
- You will notice that the `Source / Import Options` pane is now populated with a selection of configuration options

### Source / Import Options

- `Keysafe Key` - Choose the [Keysafe Key that you created above](#authentication)
- `Customer ID` - Optional Customer ID of the Google Workspace account. If not provided, `my_customer` will be set, as per the [Google Workspace Directory API definition](https://developers.google.com/workspace/admin/directory/v1/guides/manage-users#get_all_users)
- `Domain` - Optional Primary Domain Name to retrieve users from, as per the [Google Workspace Directory API definition](https://developers.google.com/workspace/admin/directory/v1/guides/manage-users#get_all_domain_users)
- `Query` - The optional Query string allows for the searching of users over many fields in a user profile, including both core and custom fields. See the [Google Search for Users documentation](https://developers.google.com/workspace/admin/directory/v1/guides/search-users) for examples
::: warning
If you do not provide a query, then ALL user objects from your Google Workspace domain will be returned and imported!
:::
- `Return Deleted User` - Returns deleted users as part of the API call into the Google Workspace Directory API 
- `Action` - This allows you to specify which action to take when importing discovered users:
    - `Create` - This will create any users found where no matching user pre-exists in your Hornbill instance. Imports defined with this action will not perform any updates on existing users matched in the Hornbill instance
    - `Update`  - This will update any users found where a matching user pre-exists in your Hornbill instance. Imports defined with this action will not create any new users where they are not matched with existing users in the Hornbill instance
    - `Create & Update` - This will perform both `Create` and `Update` actions, so will create new users where no matching user is found in the Hornbill instance, and will update any Hornbill user records when a match is found
- `Account Status` - This allows you to specify the Account Status for the imported users, and when the Account Status should be applied
- `User Properties` - This allows you to define which Hornbill User Properties should be populated during Create and Update actions. The string inputs for the user property fields support the use Mustache templates for [data mapping and manipulation](/data-imports-guide/cloud-users/data-mapping), and contain an auto-complete feature to present a list of mappable fields from the source data
- `Memberships` - A list of organizational groups who the imported users should be made members of:
    - `Action` - The triggering action:
        - `Create` - Set group membership during User Create actions only. If matched user already exists and we’re performing an update, do not add user to group
        - `Update` - Set group membership during User Update actions only
        - `Create & Update` - Set group membership during both User Create and User Update actions
        - `Unassign if Assigned` - If the user exists, and is already a member of the group, then remove them from the group
    - `Organization ID` - The Id of the organizational group
    - `Membership` - The membership of the group, can be one of:
        - `Member`
        - `Team Leader`
        - `Manager`
    - `Can View Tasks` - Can the user see group tasks
    - `Can Action Tasks` - Can the user action group tasks
    - `Single Assignment per Group Type` - When set, the users will be associated with this group, and any other organizational group memberships of the same group type will be removed from the user
- `Role Assignments` - A list of roles to assign to the imported users
    - `Action` - The triggering action:
        - `Create` - Assign role during User Create actions only. If matched user already exists and we’re performing an update, do not assign role to user
        - `Update` - Assign role during User Update actions only
        - `Create & Update` - Assign role during both User Create and User Update actions
        - `Unassign if Assigned` - If the user exists, and is already role, then remove the role from the user
    - `Role` - The name of the role to assign to the user