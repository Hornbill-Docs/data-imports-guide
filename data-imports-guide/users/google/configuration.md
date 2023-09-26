# Configuration

## Overview

The options and configuration of imports using this utility are saved in one or more [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) files in the folder where the utility executable resides.

We have provided a default configuration file in each of the downloadable archives: `conf.json`. This can be used as a template from which to build your import configurations.

:::tip
The utility will default to `conf.json` if a configuration file is not specified as a command line argument
:::

## Example Configuration

```json
{
  "GoogleConf": {
    "KeysafeID": 0,
    "Customer": "",
    "Domain": "yourdomain.com",
    "Query": "name:'steve'",
    "ShowDeleted": false
  },
  "User": {
    "Operation": "Both",
    "HornbillUserIDColumn": "h_user_id",
    "AccountMapping": {
      "UserType": "user",
      "UserID": "{{.primaryEmail}}",
      "LoginID": "{{.primaryEmail}}",
      "EmployeeID": "{{.employeeId}}",
      "Name": "{{.fullName}}",
      "FirstName": "{{.givenName}}",
      "LastName": "{{.familyName}}",
      "JobTitle": "{{.jobTitle}}",
      "Phone": "{{.workPhone}}",
      "Email": "{{.primaryEmail}}",
      "Mobile": "{{.mobilePhone}}",
      "Language": "{{.languageCode}}",
      "AbsenceMessage": "",
      "TimeZone": "",
      "DateTimeFormat": "",
      "DateFormat": "",
      "TimeFormat": "",
      "CurrencySymbol": "",
      "CountryCode": ""
    },
    "Type": {
      "Action": "Both"
    },
    "Status": {
      "Action": "Both",
      "Value": "active"
    },
    "Role": {
      "Action": "Both",
      "Roles": [
        "Basic User Role"
      ]
    },
    "ProfileMapping": {
      "Manager": "{{.managerEmail}}",
      "MiddleName": "",
      "JobDescription": "{{.employeeType}}",
      "Qualifications": "",
      "Interests": "",
      "Expertise": "",
      "Dob": "",
      "Nationality": "",
      "Religion": "",
      "HomeTelephone": "{{.homePhone}}",
      "SocialNetworkA": "",
      "SocialNetworkB": "",
      "SocialNetworkC": "",
      "SocialNetworkD": "",
      "SocialNetworkE": "",
      "SocialNetworkF": "",
      "SocialNetworkG": "",
      "SocialNetworkH": "",
      "PersonalInterests": "",
      "HomeAddress": "{{.homeAddress}}",
      "PersonalBlog": "",
      "Attrib1": "{{.creationTime}}",
      "Attrib2": "{{.gender}}",
      "Attrib3": "{{.lastLoginTime}}",
      "Attrib4": "{{.newEmployeeType}}",
      "Attrib5": "",
      "Attrib6": "",
      "Attrib7": "",
      "Attrib8": ""
    },
    "Image": {
      "Action": "Both"
    },
    "Site": {
      "Action": "Both",
      "Value": "{{.buildingId}}"
    },
    "Org": [
      {
        "Action": "Both",
        "Value": "{{.department}}",
        "Options": {
          "Type": "department",
          "Membership": "member",
          "TasksView": false,
          "TasksAction": false,
          "OnlyOneGroupAssignment": true
        }
      },
      {
        "Action": "Both",
        "Value": "{{.costCenter}}",
        "Options": {
          "Type": "costcenter",
          "Membership": "member",
          "TasksView": false,
          "TasksAction": false,
          "OnlyOneGroupAssignment": true
        }
      },
      {
        "Action": "Both",
        "Value": "Some Google Company",
        "Options": {
          "Type": "company",
          "Membership": "member",
          "TasksView": false,
          "TasksAction": false,
          "OnlyOneGroupAssignment": true,
          "SetAsHomeOrganisation": true
        }
      }
    ]
  },
  "Actions": [
    {
      "Action": "Replace",
      "Value": "{{.employeeType}}",
      "Output": "newEmployeeType",
      "Options": {
        "ReplaceOld": "String to replace",
        "ReplaceNew": "String to replace the above with"
      }
    }
  ],
  "Advanced": {
    "LogLevel": 0,
    "LogRetention": 7
  }
}
```

### Configuration Explanation

* `GoogleConf`
  * `KeysafeID` - Type: `integer` - The ID of the Keysafe Key that the utility will use to authenticate all Google Workspace requests with. See the [[Google Workspace]] page for more information regarding creating this key.
  * `Customer` - Type: `string` - The unique ID for the customer's Google Workspace account. In case of a multi-domain account, to fetch all groups for a customer, fill this field instead of **Domain**. You can also use the **my_customer** alias to represent your account's **Customer** ID. Either the **Customer** or **Domain** property must be provided.
  * `Domain` - Type: `string` - The domain name. Use this field to get groups from only one domain. To return all domains for a customer account, use the **Customer** property instead. Either the **Customer** or **Domain** property must be provided.
  * `Query` - Type: `string` - Query string for searching user fields. For more information on constructing user queries, see the [Search for Users Google Documentation](https://developers.google.com/admin-sdk/directory/v1/guides/search-users).
  * `Show Deleted` - Type: `boolean` - If set to `true`, the API call to Google will return information about any deleted accounts. This is particularly useful for processing leavers, and setting their Hornbill accounts to archived.
* `User`
  * `Operation` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform on the discovered user records
  * `HornbillUserIDColumn` - Type: `string` - The column to match Google users against Hornbill users
  * `AccountMapping` - Type: `object` - The Hornbill user account properties to match your discovered Google user record fields against
  * `ProfileMapping` - Type: `object` - The Hornbill user extended profile properties to match your discovered Google user record fields against
  * `Type`
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account type updates on the discovered user records
  * `Status`
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account status updates on the discovered user records
    * `Value` - Type: `string` - Can be `active`, `suspended` or `archived`
  * `Role`
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account roles on the discovered user records
    * `Roles` - Type: `array` - A list of names of roles to apply to the discovered users
  * `Image`
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account profile image updates on the discovered user records
  * `Site`
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account profile site updates on the discovered user records
    * `Value` - Type: `string` - Mappable name of the site to apply to the discovered users records 
  * `Org` - Type: `array` - Organizational units to associate the imported users with:
    * `Action` - Type: `string` - Can be `both`, `create` or `update` - import actions to perform account organizational unit updates on the discovered user records
    * `Value` - Type: `string` - Mappable name of the organizational unit to apply to the discovered users records
    * `Options` - Type: `object` - A collection of properties to define membership settings of organizational group:
      * `Type` - Type: `integer` - The numeric value for the group type (team = 1, company = 5, etc). See the [group type datatype documentation](/esp-api/types/simple/groupType) for supported values.
      * `Membership` - Type: `string` - The group membership role, can be one of `member`, `teamLeader` or `manager`.
      * `TasksView` - Type: `boolean` - If set to `true`, then the imported users can view tasks assigned to this group (Typically only required for Groups of type "Team").(Typically only required for Groups of type "Team").
      * `TasksAction` - Type: `boolean` -  If set to `true`, then the user can action tasks assigned to this group.
* `Actions` - a list of pre-import actions to perform on discovered user properties
  * `Action` - Can be `Regex`, `Replace` or `Trim` - the action to be performed
  * `Value` - The value to perform the action of. Can be hard-coded or mapped with template variables
  * `Output` - the name of the new property to store the action output in. These are placed in the discovered user data, and can then be mapped to using templates in both the **AccountMapping** and **ProfileMapping** objects
  * `Options` - The options that can be set for the action - 
    * `ReplaceOld` - Action `Replace` - The old string to be replaced from the Value string
    * `ReplaceNew` - Action `Replace` - The string to replace the old string from above with, from the Value string
    * `RegexValue` - Action `Regex` - The regular expression to perform against the Value
* `Advanced` 
  * `LogLevel` - The log level to apply to the output
  * `LogRetention` - The number of days to keep logs for

#### Associating a Site to Hornbill User Accounts

This utility has the ability to associate a Hornbill Site record to a user account based on the contents of a field. This is achieved through a look-up, the mechanism of which is quite simple and works in the following manner: 

* The import reads the fields (template rules work) that is specified in the `Value` field. In the example shown, the `site` field is used.
* It takes the content and tries to identify if there is an existing site record in Hornbill with a name that matches the value of the site. e.g. if the site field contained **Brussels**, the import would look for a Hornbill Site record with the name **Brussels**.
* If a match is found, the import will associate the user to the site.
* If no site record is found, the import will move onto the next user.

:::note
The name of the Site record in Hornbill must match the value of the directory attribute specified.
:::

#### Associating a Group to Hornbill User Accounts

This utility has the ability to associate a Hornbill Organizational Group to a user account based on the contents of a fieldname. This is achieved through a look-up, the mechanism of which is quite simple and works in the following manner:

* The utility reads the attribute that is specified in the `Value` section. In the example shown, the department field is used.
* It takes the content and tries to identify if there is a Hornbill Group that exists with a name that matches the value of the field name. e.g. if the department field contained **Accounting**, the utility would look for a Hornbill Organizational Group called **Accounting**.
* If a match is found, the import will associate the user to the group.
* If no Hornbill organization is found, the import will move onto the next user.

:::note
The name of the Organization(Group) in Hornbill must match the value of the directory attribute specified.
:::

### Google Workspace - Available Properties

Aside from the standard set of properties provided by Google (see the [Google User object documentation](https://developers.google.com/admin-sdk/directory/reference/rest/v1/users#User) for more information), we also provide a list of other useful properties that you can use in your templates:

* Extended (or flattened) user properties:
  * `familyName`
  * `fullName`
  * `givenName`
  * `homePhone`
  * `workPhone`
  * `mobilePhone`
  * `managerEmail`
  * `jobTitle`
  * `employeeId`
  * `employeeType`
  * `languageCode`
  * `customAddress`
  * `homeAddress`
  * `otherAddress`
  * `workAddress`
* Location type properties:
  * `buildingId`
  * `customType`
  * `deskCode`
  * `floorName`
  * `floorSection`