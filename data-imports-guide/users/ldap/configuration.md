# Configuration

## Overview

The options and configuration of imports using this utility are created, updated and stored on your Hornbill instance. The utility will connect to your instance and retrieve the configuration options via the Hornbill web services API for use during each individual import run, storing the options, and any required credentials, in memory and not on the local disk.

## Configuration

To access the configuration options for this utility, on your Hornbill instance navigate to `Configuration` > `Platform Configuration` > `Data Import Configurations`. From here, you can create, view, duplicate or delete import configuration definitions.

LDAP data import configurations are split into the following sections:

### Details

This section contains the basic information regarding an import - `Name`, `Type` and `Description`,

<img src="/_books/data-imports-guide/images/ldap_user_import_details.png" width="650px" alt="Details Example"/>

### Docs

This section is read-only, and contains useful information and links pertinent to the running and configuration of the utility.

### Command Line

This section contains the key information required to execute the import, and will generate example commands that can be entered into your Windows command-line, PowerShell and/or Task Scheduler to run your imports.

<img src="/_books/data-imports-guide/images/ldap_user_import_commandline.png" width="650px" alt="Comand Line Example"/>

### History

This section presents a list of historical imports performed using the specific configuration. Click on any of the historic import records here to be presented with more detailed information about the specific import run.

<img src="/_books/data-imports-guide/images/ldap_user_import_history.png" width="650px" alt="History Example"/>


### LDAP Server

This section is where the information required to set up communication with your directory service (i.e. Active Directory) is defined.

#### LDAP Server

This sub-section allows you to define the authentication and connection information for your LDAP service.

<img src="/_books/data-imports-guide/images/ldap_user_import_server.png" width="650px" alt="Server Example"/>

* `Authentication` - Reference to the Hornbill KeySafe entry that securely stores the username, password, and port used during the connection to your directory server.
* `Connection Type` - The type of HTTP connection to use when communicating with the directory Server. Normal HTTP, SSL, and TLS are supported.
* `Allow Insecure Connection` - Used in conjunction with SSL or TLS connection types and allows the verification of SSL Certifications to be disabled i.e. `ON`` sets the InsecureSkipVerify variable to `true``.
* `Enable Debug` - Enables LDAP Connection Debugging. This generates a significant amount of additional logging, and should only ever be enabled to troubleshoot connection issues during the initial setup and testing.

#### LDAP Query

This sub-section allows you to define the LDAP query and associated options, as well as the list of LDAP attributes that should be returned for each user being imported.

<img src="/_books/data-imports-guide/images/ldap_user_import_query.png" width="650px" alt="Query Example"/>

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

<img src="/_books/data-imports-guide/images/ldap_user_import_account.png" width="650px" alt="User Account Mapping Example"/>

The directory attributes provided by default should be enough to get you started as they are typically where you would find this information referenced in most directory service implementations. However, it is prudent to check that the attribute specified does actually hold the information in your directory as expected. `User ID` and `User Type` are required, and mandatory.

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

In addition to the fundamental User Account Properties, Hornbill can store much more information about a user.

### User Options

This section is where the ancillary associations are managed e.g Manager, Site, Role, and Group associations.

### Pre Import Actions

This section allows you to create and configure any pre-import actions that should be carried out against found LDAP attribute values before presenting them back as additional variables that can be used in your user record field mappings.   

### Advanced Options

Each time the import is run, it outputs a log file. This tab allows you to set some preferences in relation to the log output.

### Debug

This section allows you to view the raw configuration which is being drawn from your selections and entries made in the previous tabs.
