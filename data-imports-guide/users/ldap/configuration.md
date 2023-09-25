# Configuration

## Overview

The options and configuration of imports using this utility are created, updated and stored on your Hornbill instance. The utility will connect to your instance and retrieve the configuration options via the Hornbill web services API for use during each individual import run, storing the options, and any required credentials, in memory and not on the local disk.

## Configuration

To access the configuration options for this utility, on your Hornbill instance navigate to `Configuration` > `Platform Configuration` > `Data Import Configurations`. From here, you can create, view, duplicate or delete import configuration definitions.

LDAP data import configurations are split into the following sections:

### Details

This section contains the basic information regarding an import - `Name`, `Type` and `Description`,

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_details.png" width="650px" alt="Details Example"/>

### Docs

This section is read-only, and contains useful information and links pertinent to the running and configuration of the utility.

### Command Line

This section contains the key information required to execute the import, and will generate example commands that can be entered into your Windows command-line, PowerShell and/or Task Scheduler to run your imports.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_commandline.png" width="650px" alt="Comand Line Example"/>

### History

This section presents a list of historical imports performed using the specific configuration. Click on any of the historic import records here to be presented with more detailed information about the specific import run.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_history.png" width="650px" alt="History Example"/>


### LDAP Server

This section is where the information required to set up communication with your directory service (i.e. Active Directory) is defined.

#### LDAP Server

This sub-section allows you to define the authentication and connection information for your LDAP service.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_server.png" width="650px" alt="Server Example"/>

* `Authentication` - Reference to the Hornbill KeySafe entry that securely stores the username, password, and port used during the connection to your directory server.
* `Connection Type` - The type of HTTP connection to use when communicating with the directory Server. Normal HTTP, SSL, and TLS are supported.
* `Allow Insecure Connection` - Used in conjunction with SSL or TLS connection types and allows the verification of SSL Certifications to be disabled i.e. `ON` sets the InsecureSkipVerify variable to `true`.
* `Enable Debug` - Enables LDAP Connection Debugging. This generates a significant amount of additional logging, and should only ever be enabled to troubleshoot connection issues during the initial setup and testing.

#### LDAP Query

This sub-section allows you to define the LDAP query and associated options, as well as the list of LDAP attributes that should be returned for each user being imported.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_query.png" width="650px" alt="Query Example"/>

* `Filter` - Provides the ability to target specific directory objects. As a minimum `(objectClass=user)` should be specified to ensure the utility only retrieves User Objects. Many online references are available detailing the various possibilities of LDAP filter syntax. Search the web for `LDAP filter syntax`.
* `DSN` - Allows us to specify the Data Source Name. E.g. `DC=test,DC=yourdomain,DC=com`. The utility can target a single OU or the entire directory from the root. The `Scope` variable can add flexibility here however it is not possible to target a selection of specific OU's in a single import configuration. If this is required, explore the creation of further import configurations to each target a specific OU.
* `Scope` - Search Scope (ScopeBaseObject = 0, ScopeSingleLevel = 1, ScopeWholeSubtree = 2) Default is 1.
* `Dereference Aliases` - Allows us to choose at what stage during the search operation (LDAP Query) aliases are dereferenced. The default value that exists here should be suitable for most.
* `Size Limit` - Allows you to set a size limit in relation to the result set that can be returned. Setting to `0` disables this setting.
* `Time Limit` - Allows you to impose a time limit in relation to how long the LDAP query can run before timing out. Setting to `0` disables this setting.
* `Types Only` - Enabling this setting will cause the query to return attribute types (descriptions) rather than attribute values.
* `Attributes` - A list of LDAP attributes that should be returned by the LDAP query. The list of attributes that exists by default is not exhaustive, and while these are some of the most common attributes used you may find that your directory contents differ slightly. Review the Hornbill user account properties and decide which directory attribute will be used to populate that information, and then ensure the attribute is listed here. To add a new attribute to the list, click the green '+' button and type the name of the attribute in the text box that appears. To delete an attribute that you don't require, click the red trash can button alongside the attribute.

:::important 
It is necessary to list all the directory attributes that you will be using to populate the Hornbill User account properties and extended user properties. If the directory attribute is not listed here, the import won't be able to include it in the import. Only attributes specified here will be recognized by the import process - i.e. any attributes you specify in any of the subsequent tabs must be listed in this section.
:::

:::note
The `thumbnailPhoto` attribute contains binary data for the thumbnail and is a requirement if you plan to use the ImageLink (outlined in a later section). If you do NOT plan to use the ImageLink, you can leave this out and reduce the load on your AD requests.
:::

### User Account

#### Import Account Action

This section allows you to specify the type of import action should be performed on the discovered user records:

* `Both Create & Update` - The import will perform both the creation of newly discovered user records, as well as the update of existing Hornbill users who have been matched with a discovered LDAP user record.
* `Create Only` - The import will only create newly discovered user records from your LDAP query output into your Hornbill instance, and will ignore any accounts that match an existing Hornbill user record.
* `Update Only` - The import will only update discovered user records from you LDAP query output that match an existing user in your Hornbill instance, and will ignore any that do not find a match in your Hornbill user records.

#### User Account Mapping

This section contains the mapping between the Hornbill user record fields and the LDAP user object attributes. i.e. which LDAP attribute values will be used to populate the Hornbill user record fields. Only some of these fields are required.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_account.png" width="650px" alt="User Account Mapping Example"/>

The directory attributes provided by default should be enough to get you started as they are typically where you would find this information referenced in most directory service implementations. However, it is prudent to check that the attribute specified does actually hold the information in your directory as expected. `User ID` and `User Type` are mandatory fields.

Directory attributes can be quickly mapped using the attribute picker located to the right of each field. The contents of the attribute picker are derived from the list of attributes you defined in the previous `LDAP Server` tab.

General rules for the attribute mapping:

* Any value wrapped with `[]` will be treated as a directory attribute.
* Any value wrapped with `{}` will be treated as an attribute that has been processed and output by a `Pre Import Action`, documented later this this article.
* Any value of `__clear__` will clear the target column for user records being updated.
* It is possible to construct a value by specifying multiple directory and/or Pre Imnport attributes against a Hornbill user property, for example:
   * `[givenName] [sn]` - Both Variables are evaluated from LDAP and set to the specified parameter.

There are a number of field-specific attribute mapping rules too:

* `Password` - The password field can. and should, be left empty as the utility generates a secure password that adheres to the User Password Policy as specified on your Hornbill instance. This password will only be temporary as the user should use the "Forgot Password" link available on the Hornbill Login Screen to reset their password the first time they navigate to your Hornbill instance. If necessary during testing, this can be populated to apply a specific password to imported accounts.
* `Site` - This can be populated with the unique key (integer) of a Hornbill Site. Note - if you plan on using the `Site Lookup` mechanism described later in this article, then this can be left blank, as this can make a site association based on the contents of a directory attribute.
* `User Type` - This defines if a user is Co-Worker or Basic user and should ONLY have the value "user" or "basic".
* `Country Code` - Expects ISO 3166 Alpha 2 two Character Country Code. (Active Directory references can be found in the [Microsoft documentation](https://learn.microsoft.com/en-gb/windows/win32/ad/values-for-the-countrycode-and-c-properties)

### User Profile

Mappings to the extended Hornbill user profile properties works in exactly the same way as the User account mapping above.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_profile.png" width="650px" alt="User Profile Mapping Example"/>

### User Options

This section is where the ancillary associations are managed e.g Manager, Site, Role, and Group associations.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_options.jpg" width="650px" alt="User Options Example"/>

#### Type

This section of the import configuration allows you to define whether or not user account types should be set on account creation, account updates, or both. The best-practice approach when starting out with the Hornbill LDAP import is to set this value to `Only Create`, as this ensures the User Type is set appropriately when new accounts are created but allows you control over when an account should be promoted from Basic to Full, or demoted from Full to Basic. Setting this to `No Action` disables this section.

#### Security

This section of the import configuration allows you to define whether the user security options should be set on account creation, account updates, or both.

#### Status

This section of the import configuration allows you to define whether the user status should be set on account creation, account updates, or both. Is also allows for the status value to be set to one of the following values:

* `Active`
* `Suspended`
* `Archived`

Setting account statuses to `Suspended` or `Archived` can be useful if you are processing leavers in your LDAP, and have a specific import configuration for updating those leaver accounts in Hornbill.

#### Unique Field

This section of the import configuration allows you to define which Hornbill user account field will be used as the unique identifier when mapping against your LDAP user records.

#### Location

This section of the import configuration allows you to define whether the user location should be set on account creation, account updates, or both. Is also allows for the location value to be set as either a hard-coded value, or mapped from an LDAP (or pre-import action processed) user record attribute.

#### Roles

This section of the import configuration allows you to define which roles should be applied to the imported user records, on account creation, account updates, or both.

#### Manager

This section of the import configuration allows you to define whether the manager field should be processed and applied to the imported user records, on account creation, account updates, or both.

* `Action` - The action type:
  * `No Action` - Manager fields processing skipped.
  * `Create & Update` - Manager fields processed on both create and update user import types.
  * `Only Create` - Manager fields processed only when user records are being created, and NOT updated.
  * `Only Update` - Manager fields processed only when user records are being updated, and NOT created.
* `Value` - The mapped value, the same rules apply as the User Account Mapping rules in the [User Account](/data-imports-guide/users/ldap/configuration#user-account) section of this document.
* `Regex` - Optional regular expression to match the name from a DSN String. The following regex will work for the majority of situations: 
  * `CN=(.*?)(?:,[A-Z]+=|$)`
* `Reverse` - Reverse the name string matched from the regular expression, above.
* `Match Against DN` - Match the users manager against the Distinguished Name instead of their name.
* `Search For Manager Id` - Lookup the Hornbill User ID of the users manager using their name. The Managers Name matched in the regular expression, above, must explicitly match the full name in Hornbill.

#### Image

This section of the configuration affords the ability to upload images to User profiles in Hornbill. Only jpg/jpeg and png formats are supported.

* `Action` - The action type:
  * `No Action` - Image processing skipped.
  * `Create & Update` - Images processed on both create and update user import types.
  * `Only Create` - Images processed only when user records are being created, and NOT updated.
  * `Only Update` - Images processed only when user records are being updated, and NOT created.
* `Upload Type` - The type of image upload sequence that will be used.
  * `AD` - Use the data stored in AD - set the `URI` input as the LDAP field surrounded by square brackets, for example: `[thumbnailPhoto]`.
  * `URL` - Use the image at end of `URI` input below - assuming that the URL is available publicly on the internet, for example: `http://whatever.com/[userPrincipalName].jpg`.
  * `URI` - Use the image at end of `URI` below - from a LOCAL web host on your network, for example: `http://localserver/[userPrincipalName].jpg`.
  * `LOCAL` - Use the image within your local file storage - set up the `URI` as the file path to the image, for example: `/PathExample/UserExample/FileExample/[userPrincipalName].png`
* `Image Type` - (`jpg` | `png`)  - The type of image as stored in the URI, below.
* `URI` - referencing the image data. Set to `__clear__` to remove the profile image for user records being updated.

#### Organizations

This section of the configuration enables the association of your imported users to organizational groups in Hornbill.

The User Import - LDAP utility has the ability to associate one or more Hornbill Groups to imported user accounts based on the contents of a directory attribute. This is achieved through a **look-up**. The look-up mechanism is quite simple and works in the following manner:

* The utility reads the attribute that is specified in the orgLookup section. In the example shown, the [department] directory attribute is used.
* It takes the content and tries to identify if there is a Hornbill Group that exists with a name that matches the value of the LDAP attribute. For example, if the `[department]` directory attribute contained `Accounting`, the utility would look for a Hornbill Group called `Accounting`.
* If a match is found, the import will associate the user to the group.
* If no Hornbill organization is found, the import will move onto the next user.

:::important
The name of the Organization (group) in Hornbill must match the value of the attribute in LDAP.
:::

* `Action` - The action type:
  * `No Action` - Image processing skipped.
  * `Create & Update` - Images processed on both create and update user import types.
  * `Only Create` - Images processed only when user records are being created, and NOT updated.
  * `Only Update` - Images processed only when user records are being updated, and NOT created.
* `Value` - The directory attribute that contains the company or division or department name to use for the Hornbill Group Look-up.
* `Member Of` - This field makes reference to the "memberOf" attribute of the user object in your directory. When a distinguished name (DN) of an LDAP group is specified in this field, the utility will only allow the group association in Hornbill if the group DN exists in the `memberOf` attribute of the user object being processed. i.e. only allow the Hornbill group association if the user object is a member of this LDAP group.
* `Type` - The Hornbill Group type (General/Team/Function/Department/Costcenter/Division/Company/Business Unit/Directorate/Branch/Board/Subsiduary). This focuses the import to only perform the look-up on Groups of a particular type (this has no relevance to the contents of your directory or the directory attribute that is used).
* `Membership` - The Hornbill Group Membership the users will be added with. One of `Member`, `Team Leader` and `Manager`.
* `Can View Tasks` - If set to `ON`, then the user can view tasks assigned to this group (only used for Groups of type `Team`).
* `Can Action Tasks` - If set to `ON`, then the user can action tasks assigned to this group (only used for Groups of type `Team`).
* `Only One Group Assignment` - When set to `ON`, a user can only be associated to a single group at any one time. i.e. if in the latest import the configuration was set to associate users to the Accounting department, the users would be associated to this new department and removed from any other department-type group that they were already associated with.
* `Set As Home Organization` - Only visible when the Type is Company. When set to `ON`, the Company will be set as the users Home Organization.

:::note
A successful association of a user to a Group is dependant upon the import finding a Hornbill Group with a name that matches the contents of the specified user attribute in your directory.
:::

Users can be associated to more than one group during the same import. Click the blue **+** button to add another group.

### Pre Import Actions

This section allows you to create and configure any pre-import actions that should be carried out against LDAP attribute values before presenting them back as additional variables that can be used in your user record field mappings. Pre Import Avtion   

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_preaction.jpg" width="650px" alt="Pre Import Example"/>

* `Value` - The directory attribute, or hard-coded value, that should be processed.
* `Action` - The action that should be performed against the above value. One of:
  * `No Action` - Returns the value with no transformations performed.
  * `Regex` - Performs a regular expression search against the `Value`, returns the match:
    * `Regex Value` - The regular expression.
  * `Replace` - Performs a find & replace on the `Value`:
    * `Find` - The string to search for.
    * `Replace With` - The string to replace the searched string with.
  * `Trim` - Trims any whitespace characters from before and after the mapped `Value`.
  * `LDAP Timestamp to Date & Time` - Converts the `Value` from the LDAP timestamp format, into a `YYYY-MM-DD hh:mm:ss` datetime format.
  * `Object SID Conversion` - Converts the `Value` from the binary Object SID format, into a human-readable Object SID.
  * `GUID Conversion` - Converts the `Value` from the binary GUID format, into a human-readable GUID.
* `Output` - The name of the new variable that is created, that can be used in import mappings across the User Account, User Profile and User Options import definitions. Note - the new, pre-processed variables use curly braces when mapping into user fields, rather than the LDAP attribute mappings that are wrapped with square brackets. From the above image, the provided values would be mapped as follows:
  * `[department]`
  * `{trimmed_department}`
  * `[telephoneNumber]`
  * `{phone}`

### Advanced Options

 This section allows you to set some preferences in relation to the log output, as well as the page size when caching records from your Hornbill instance.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_advanced.jpg" width="650px" alt="Advanced Options Example"/>

* `Log Level` - The minimum level of log entry to output. Note, `Debug` will output a significantly higher number of log entries than the other options, and should only be used when debugging LDAP output and mapping issues with your imports:
  * `Debug` - Outputs LDAP query inputs and outputs, as well as all other log entry types as below.
  * `Message` - Outputs standard messages, warnings and errors to the local log.
  * `Warning` - Only outputs warnings and errors to the local log.
  * `Error` - Only outputs errors to the local log.
* `Log Retention (Days)` - The number of days to keep you local log files for.
* `Page Size` - The number of records to return for each page of cache data.

### Debug

This section allows you to view the raw configuration which is created from your selections and entries made in the previous tabs.

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_debug.jpg" width="650px" alt="Debug Example"/>