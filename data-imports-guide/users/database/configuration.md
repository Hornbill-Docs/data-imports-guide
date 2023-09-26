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
  "Database": {
    "Source": "mysql",
    "Encrypt": false,
    "Query": "SELECT userdb.*, b.fullname AS manager FROM userdb LEFT JOIN userdb b ON b.keysearch = userdb.fk_manager"
  },
  "Action": "Both",
  "User": {
    "UserDN": "{{.keysearch}}",
    "HornbillUniqueColumn": "h_user_id",
    "AccountMapping": {
      "UserID": "{{.keysearch}}",
      "LoginID": "{{.keysearch}}",
      "EmployeeID": "{{.keysearch}}",
      "UserType": "basic",
      "Name": "{{.firstname}} {{.surname}}",
      "Password": "",
      "FirstName": "{{.firstname}}",
      "LastName": "{{.surname}}",
      "JobTitle": "",
      "Site": "{{.site}}",
      "Phone": "{{.telext}}",
      "Email": "{{.email}}",
      "Mobile": "[mobile]",
      "AbsenceMessage": "",
      "TimeZone": "",
      "Language": "",
      "DateTimeFormat": "",
      "DateFormat": "",
      "TimeFormat": "",
      "CurrencySymbol": "",
      "CountryCode": "",
      "Enable2FA": "",
      "DisableDirectLogin": "",
      "DisableDirectLoginPasswordReset": "",
      "DisableDevicePairing": ""
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
      "MiddleName": "",
      "JobDescription": "",
      "Manager": "{{.manager}}",
      "WorkPhone": "",
      "Qualifications": "",
      "Interests": "",
      "Expertise": "",
      "Gender": "",
      "Dob": "",
      "Nationality": "",
      "Religion": "",
      "HomeTelephone": "{{.telext}}",
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
      "Attrib1": "1",
      "Attrib2": "2",
      "Attrib3": "3",
      "Attrib4": "4",
      "Attrib5": "5",
      "Attrib6": "6",
      "Attrib7": "7",
      "Attrib8": "8"
    },
    "Manager": {
      "Action": "Both",
      "Value": "{{.manager}}",
      "Options": {
        "GetStringFromValue": {
          "Regex": "",
          "Reverse": false
        },
        "MatchAgainstDistinguishedName": false,
        "Search": {
          "Enable": true,
          "SearchField": ""
        }
      }
    },
    "Image": {
      "Action": "Both",
      "UploadType": "URL",
      "InsecureSkipVerify": false,
      "ImageType": "jpg",
      "URI": "http://sample.myservicedesk.com/sw/clisupp/documents/userdb/images/{{.keysearch}}.jpg"
    },
    "Site": {
      "Action": "Both",
      "Value": "{{.site}}"
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
      },
      {
        "Action": "Both",
        "value": "{{.companyname}}",
        "MemberOf": "",
        "Options": {
          "Type": 5,
          "Membership": "member",
          "TasksView": false,
          "TasksAction": false,
          "OnlyOneGroupAssignment": false,
          "SetAsHomeOrganisation": true
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

- `KeysafeKeyID` - Type: `integer` - The ID of the KeySafe Key that contains your database authentication details. See the [KeySafe section of the Authentication article](/data-imports-guide/users/database/authentication#keysafe) for more information.
- `Database` - Type: `object` - The information that the utility needs to query your source database.
  - `Source` - Type: `string` - The database technology the utility needs to connect to, one of:
    - `mssql` - Microsoft SQL Server
    - `mysql` - MySQL or MariaDB
    - `csv` - CSV file, via ODBC
    - `excel` - Excel spreadsheet, via ODBC 
  - `Encrypt` - Type: `boolean` - Specifies whether the connection between this utility and the source database should be encrypted. There is a bug in SQL Server 2008 and below that causes connection failures if the connection is encrypted. Only set this to true if your SQL Server supports this, or has been patched accordingly.
  - `Query` - Type: `string` - The SQL query to return your list of users for import. Please note that all field names are specificied as lowercase - this is to ensure smooth operation for field mapping (as they are case-sensitive). Also, by specifying each field in use, one is following SQL best practice as it prevents unnecessary/unused data to be returned, stored and processed (longvarchar and blob fields, for instance, can take up a lot of memory). For CSV please use UPPERCASE for all field names and, also, do NOT use spaces in the headers/fieldnames.
- `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. Import actions to perform on the discovered user records.
- `User` - Type: `object` - Contains all source database records to Hornbill mappings, and user configuration.
  - `UserDN` - Type: `string` - The content from the source database user records that matches the unique reference against your Hornbill user records.
  - `HornbillUniqueColumn` - Type: `string` - Can be `h_user_id`, `h_employee_id`, `h_login_id` or `h_email`. The column from the Hornbill user records that is used to match the user records against their `UserDN` (above).
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
  - `Status` - Type: `object` - Contains two properties, Import actions to perform on the Status of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - Can be `active`, `suspended`, or `archived`. 
  - `Role` - Type: `object` - Contains two properties, Import actions to perform on the Roles of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Roles` - Type: `array` - A list of roles that should be assigned to the users being imported.
  - `Manager` - Type: `object` - Contains three properties, Import actions to perform on the Manager field of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - The mapped value of the users manager.
    - `Options` - Type: `object` - Contains several properties to allow granular searching of the relevant manager records:
      - `Get String From Value` - Type: `object`
        - `Regex` - Type: `string` - Optional regular expression to match the name from a DSN String.
        - `Reverse` Type: `boolean` - Reverse the name string matched from the regular expression, above.
      - `Match Against DN` Type: `string` - Match the users manager against the Distinguished Name instead of their name.
      - `Search` - Type: `object`
        - `Enable` - Type: `boolean`
        - `SearchField` - Type: `string` - The field to search for manager records against.
  - `Image` - Type: `object` - Contains several properties, Import actions to perform on profile images of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `UploadType` - Type: `string` - Can be `URI` or `URL` - local (network) drive or HTTP(S) served images
    - `InsecureSkipVerify` - Type: `boolean` - Skip invalid certificate verification when importing images 
    - `ImageType` - Type: `string` - Image type, can be `png` or `jpg`
    - `URI` - Type: `string` - The source mapping for the image
  - `Site` - Type: `object` - Contains two properties, Import actions to perform on the Site of the discovered user records:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - The source mapping of the Site.
  - `Org` - Type: `array` - An array of objects containing several properties of Organizational Unit information to associate the imported user with:
    - `Action` - Type: `string` - Can be `Create`, `Update`, or `Both`. 
    - `Value` - Type: `string` - The mapped name of the Organizational Unit.
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