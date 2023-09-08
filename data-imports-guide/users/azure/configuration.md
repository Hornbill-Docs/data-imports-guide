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
    "KeysafeKeyID": 0,
    "AzureConf": {
        "Search": "groups",
        "UserFilter": "startswith(displayName,'Dave')",
        "UserID": "mail",
        "UserProperties": [
            "businessPhones",
            "employeeId",
            "mailNickname",
            "mail",
            "givenName",
            "surname",
            "telephoneNumber",
            "department"
        ],
        "UsersByGroupID": [
            {
                "ObjectID": "Group Object ID",
                "Name": "Group Object Name"
            },
            {
                "ObjectID": "Second Group Object ID",
                "Name": "Second Group Object Name"
            }
        ]
    },
    "User": {
        "Operation": "Both",
        "HornbillUniqueColumn": "h_user_id",
        "AccountMapping": {
            "UserID": "{{.userPrincipalName}}",
            "LoginID": "",
            "EmployeeID": "",
            "UserType": "basic",
            "Name": "{{.givenName}} {{.surname}}",
            "Password": "",
            "FirstName": "{{.givenName}}",
            "LastName": "{{.surname}}",
            "JobTitle": "",
            "Site": "1",
            "Phone": "{{index .businessPhones 0}}",
            "Email": "{{.mail}}",
            "Mobile": "",
            "AbsenceMessage": "",
            "TimeZone": "",
            "Language": "",
            "DateTimeFormat": "",
            "DateFormat": "",
            "TimeFormat": "",
            "CurrencySymbol": "",
            "CountryCode": "",
            "Enable2FA": "disabled",
            "DisableDirectLogin": "false",
            "DisableDirectLoginPasswordReset": "false",
            "DisableDevicePairing": "false"
        },
        "ProfileMapping": {
            "MiddleName": "",
            "JobDescription": "",
            "Qualifications": "",
            "Interests": "",
            "Expertise": "",
            "Gender": "",
            "Dob": "",
            "Nationality": "",
            "Religion": "",
            "HomeTelephone": "{{.telephoneNumber}}",
            "SocialNetworkA": "",
            "SocialNetworkB": "",
            "SocialNetworkC": "",
            "SocialNetworkD": "",
            "SocialNetworkE": "",
            "SocialNetworkF": "",
            "SocialNetworkG": "",
            "SocialNetworkH": "",
            "PersonalInterests": "",
            "homeAddress": "",
            "PersonalBlog": "",
            "Attrib1": "",
            "Attrib2": "",
            "Attrib3": "",
            "Attrib4": "",
            "Attrib5": "",
            "Attrib6": "",
            "Attrib7": "",
            "Attrib8": ""
        },
        "Type": {
            "Action": "Both"
        },
        "Security": {
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
        "Manager": {
            "Action": "Both",
            "AzField": "userPrincipalName",
            "HornbillUniqueColumn": "h_user_id"
        },
        "Image": {
            "Action": "Both",
            "UploadType": "AZURE",
            "InsecureSkipVerify": false,
            "ImageType": "png",
            "ImageSize": "504",
            "URI": "{{.id}}"
        },
        "Site": {
            "Action": "Both",
            "Value": "{{.physicalDeliveryOfficeName}}"
        },
        "Org": [
            {
                "Action": "Both",
                "value": "{{.department}}",
                "MemberOf": "",
                "Options": {
                    "Type": 2,
                    "Membership": "member",
                    "TasksView": false,
                    "TasksAction": false,
                    "OnlyOneGroupAssignment": false,
                    "SetAsHomeOrganisation": false
                }
            }
        ],
        "Actions": [
            {
                "Action": "Trim",
                "Value": "{{.department}}",
                "Output": "department"
            },
            {
                "Action": "Replace",
                "Value": "{{.mobile}}",
                "Options": {
                    "replaceFrom": "0",
                    "replaceWith": "+44"
                },
                "Output": "phone"
            },
            {
                "Action": "Regex",
                "Value": "{{.telephoneNumber}}",
                "Options": {
                    "regexValue": "([0-9])\\w+/g"
                },
                "Output": "areaCode"
            }
        ]
    }
}
```

### Configuration Explanation

- `KeysafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your Azure authentication details. See the [KeySafe section of the Authentication article](/data-imports-guide/users/azure/authentication#keysafe) for more information.
- `AzureConf` - Type: `object` - The information that the utility needs to query Entra ID.
  - `Search` - Type: `string` - Can be `users` or `groups`. See `UserFilter` and `UsersByGroupID` below.
  - `UserFilter` - Type: `string` - Allows you to apply a filter against the list of users returned by Entra ID for processing, when the `Search` property, above, is set to `users`. See the [Microsoft API documentation](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0&tabs=http#example-7-use-search-to-get-users-with-display-names-that-contain-the-letters-wa-or-the-letters-ad-including-a-count-of-returned-objects) for more information.
  - `UserProperties` - Type: `array` - A list of fields that the import requires from Entra ID. See the [Microsoft API documentation on the $select keyword](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0&tabs=http#optional-query-parameters) for more information.
  - `UsersByGroupID` - Type: `array` - Contains a list of objects that contain Entra ID Group information. Users who are members of these Groups will be included in the import, when the `Search` property, above, is set to `groups`.
- `User` - Type: `object` - Contains all Entra ID to Hornbill mappings, and user configuration.
  - `Operation` - Type: `string` - Can be `Create`, `Update`, or `Both`. Import actions to perform on the discovered user records.
  - `AccountMapping` - Type: `object` - Data mapping, in the format `"fieldInHornbill": "data to insert"`. The data to insert can either be hard-coded strings, mapped from the source data using one or more [Go templates](https://pkg.go.dev/text/template), or a mixture of the two. Some specific examples:
    - `"UserType": "basic"` - All records will be created as Basic Users in Hornbill. See the [Hornbill user documentation](/esp-fundamentals/security/account-types#user-account-types) for more information.
    - `"Password": ""` - if the Password data mapping is left empty, then the utility with generate a random password to create new users with. The generated passwords will obey the [password policies](/esp-config/security/password-policies) defined on the target Hornbill instance. 
    - `"Site": "1"` - If set, see the comments below on SiteLookup
    - `TimeZone` - See the [list of timezones supported by Hornbill](/data-imports-guide/timezones) for valid values.
    - `Language` - Language localization tags, ISO 629 in combination with ISO 3166, examples:
      - `en-GB`
      - `en-US`
      - `fr-FR`
      - etc...
    - `DateTimeFormat` - See the [list of supported DateTime formats](/data-imports-guide/date-time-formats) for valid values.
    - `CountryCode` - ISO 3166 Alpha 2 two Character Country Code.
  - `ProfileMapping` - Type: `object` - Data mapping for user profile fields, in the format `"fieldInHornbill": "data to insert"`. See `AccountMapping` above for details.
  - `Type` - Type: `object` - Contains a single property `Action`, which can be `Create`, `Update`, or `Both`. Import actions to perform on the Type field of the discovered user records.
  - `Security` - Type: `object` - Contains a single property `Action`, which can be `Create`, `Update`, or `Both`. Import actions to perform on the Security fields of the discovered user records.
  - `Status` - Type: `object` - Contains two properties, Import actions to perform on the Status of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - Can be `active`, `suspended`, or `archived`. 
  - `Role` - Type: `object` - Contains two properties, Import actions to perform on the Roles of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Roles` - Type: `array` - A list of roles that should be assigned to the users being imported.
  - `Manager` - Type: `object` - Contains three properties, Import actions to perform on the Manager field of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `AzField` - Type: `string` - The Entra ID field to identify the manager ID against.
    - `HornbillUniqueColumn` - Type: `string` - The Hornbill field to identify the imported users' manager ID against.
  - `Image` - Type: `object` - Contains several properties, Import actions to perform on profile images of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `UploadType` - Type: `string` - Can be `URI`, `URL`, or `AZURE`, local (network) drive or HTTP(S) served image
    - `InsecureSkipVerify` - Type: `boolean` - Skip invalid certificate verification when importing images 
    - `ImageType` - Type: `string` - Image type, can be `png` or `jpg`
    - `ImageSize` - Type: `integer` - The image size to return from Entra ID, one of 48/64/96/120/240/360/432/504/648. See the [Microsoft documentation](https://learn.microsoft.com/en-us/graph/api/profilephoto-get?view=graph-rest-1.0&tabs=http) for more information and options.
    -`URI` - Type: `string` - The source mapping for the image
  - `Site` - Type: `object` - Contains two properties, Import actions to perform on the Site of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - The source mapping of the Site.
  - `Org` - Type: `array` - An array of objects containing several properties of Organizational Unit information to associate the imported user with:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - The mapped name of the Organizational Unit.
    - `MemberOf` - Type: `string` - This field references the "memberOf" attribute of the user object in your Entra ID directory. When the distinction of a group is specified in this field, the utility will only allow the group association in Hornbill if the group DN exists in the `memberOf` attribute of the user object being processed. i.e. only allow the Hornbill group association if the user object is a member of this directory group.
	- `Options` - Type: `object` - A collection of properties to define membership settings of organizational group:
	  - `Type` - Type: `integer` - The numeric value for the group type (team = 1, company = 5, etc). See the [group type datatype documentation](/esp-api/types/simple/groupType) for supported values.
	  - `Membership` - Type: `string` - The group membership role, can be one of `member`, `teamLeader` or `manager`.
	  - `TasksView` - Type: `boolean` - If set to `true`, then the imported users can view tasks assigned to this group (Typically only required for Groups of type "Team").(Typically only required for Groups of type "Team").
	  - `TasksAction` - Type: `boolean` -  If set to `true`, then the user can action tasks assigned to this group.
	  - `OnlyOneGroupAssignment` - Type: `boolean` - When set to `true`, a user can only be associated with a single group of the specified type at any one time. So for example, if in the latest import the configuration was set to associate users to the Accounting department, the users would be associated with this new department and removed from any other department group that they were already associated with.
	  - `SetAsHomeOrganisation` - Type: `boolean` - Only used when the group type is Company (5). When set to `true`, the Company will be set as the users Home Organization.
  - `Actions` - Type: `array` - A list of transform actions that can be performed on mapped field values:
    - `Action` - Type: `string` - One of `Trim`, `Replace` or `Regex`.
      - `Trim` - Trims whitespace characters from the beginning and end of the value.
      - `Replace` - Replaces part of a value string with another string.
      - `Regex` - Searches the value for content using a provided regular expression, and returns that value.
    - `Options` - Type: `object` - A collection of properties to control either the `Replace` or `Regex` actions:
      - `replaceFrom` - Type: `string` - Used in the `Replace` action. The old string that should be replaced.
      - `replaceWith` - Type: `string` - Used in the `Replace` action. The new string to replace the old string with.
      - `regexValue` - Type: `string` - Used in the `Regex` action. The regular expression to use to search the value.