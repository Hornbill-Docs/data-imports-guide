---
layout: article
keywords: copy import 
---

# Setting up your Entra ID user import

## Before you begin

* Create a [KeySafe entry for Entra ID](/data-imports-guide/cloud-users/overview#setting-up-a-keysafe-key)
* Review the [default fields](/data-imports-guide/cloud-users/entraid#about-default-fields) that Hornbill brings in for each user when you do an import.

    > **Note**: For admins familiar with Mustache templating and Microsoft Graph User resource, this article also provides information about an advanced feature: [mapping fields using the memberOf property](/data-imports-guide/cloud-users/entraid#advanced-field-mapping-with-the-memberof-property).

## Creating an import configuration

### Accessing Cloud Data Imports

1. On your keyboard use `Shift` + `Ctrl` + `s` to open the configuration search.
1. Type **Cloud data**.
1. Select **Cloud Data Imports** from the results.

or

* Navigate to **Configuration > Platform Configuration > Data > Cloud Data Imports**.

### Add a new import configuration

1. If this is your first import configuration, select the option that says **No import configurations are set up. Click here to create your first import**.  Otherwise, at the top right of the configurations list, click **+ Add New**.
2. Give your import a clear and unique name.
3. Select **Create**.
4. The form for the new import will be displayed.
5. Under the **Details** section you can optionally add a **Description** to explain the purpose of the import.
6. Make note of the **Import Enabled** toggle.  Before an import can run, you will need to toggle this to be enabled.

> **Note:** The **Save Changes** button will remain inactive until all mandatory **Data Source** fields have been filled.

## Configuring the Data Source

### Choose a data source

1. Select the **Data Source** section.
2. In the **Import** field, select the edit icon.
3. In the Hornbill Integration Bridge dialog, select **Cloud Data Imports** > **Users** > **Entra ID**.
4. Click **Apply**.
5. The **Source / Import Options** section appears, with settings you can customize.

### Credentials settings

1. In the **Data Source** section, locate the **Credentials Settings** section.
1. From the **Entra ID User Import** drop-down, select the KeySafe entry that you created for this import. If this drop-down is empty, you must go back and create your [KeySafe](/data-imports-guide/cloud-users/overview#setting-up-a-keysafe-key) entry before continuing.

> **Note**:  The **Credentials Settings** section is only available after a data source has been selected.

### Source/Import Options

This section covers the options available when setting up your Entra ID user import. These settings give you control over what gets imported.

> **Note**:  The **Source/Import Options** section is only available after a data source has been selected.

#### Mandatory fields

Before you can save the Source/Import Options, the following mandatory fields must be completed:

* Action: Create, Update, or Create and Update.
* Account Status
  * Action: Create or Create and Update.
  * User Status: Active, Suspended, or Archived.
* User Properties
  * User Type: Basic, Full, or Managed Service Provider.
  * User Type Action: Create or Create and Update.
  * User ID
  * Handle
* Profile Picture: No processing, update, or clear.

#### Query

This is where you can enter a query to limit which users are imported.

::: caution
If this field is empty, and there are no **Group ID filters** set, then *all users* from your Entra ID account will be processed.
:::

* Hornbill uses Microsoft’s [Graph API](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0&tabs=http) to access Entra ID.
* You can write a query using OData to select specific users based on things like department, email domain, etc.

Here are a few examples:

||Description|Query|
|---|---|---|
|By Department|Import users from the **Human Resources** department|`department eq 'Human Resources'`|
|By Email Domain|Import users that have an email address ending in **@hornbill.com**|`endswith(mail,'@hornbill.com')`|
|By Email Domain and Department|Import users that have an email address ending in **@hornbill.com** and from the **Human Resources** department|`endswith(mail,'@hornbill.com') and department eq 'Human Resources'`|

Resources to help you write queries:

* [$filter syntax guide](https://learn.microsoft.com/en-us/graph/filter-query-parameter?tabs=http)
* [List of available user fields](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties)

::: important
Not all properties from the [Microsoft Graph User Resource type](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0) can be used in query filters, and a number of these properties do not support the full range of OData operators to query against. See the [list of available user fields](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties) for a full list of properties, which details which of the properties can be used in the query filters, and which OData operators are supported by each.

For example:

* The value of `onPremisesDistinguishedName` can be returned and mapped into your Hornbill user records, but the field *cannot* be used in a query filter.
* The value of `onPremisesUserPrincipalName` can be returned and mapped into your Hornbill user records, and *does* support filtering using the documented OData query operators.
:::

#### Group ID filter

This is where you can provide additional Group IDs, to further filter the list of users returned by your optional Query (as detailed above).

This is a list of group identifiers, and if each user returned by the query is a member of at least one of the groups, then that user will be processed by the data import job. The following Entra ID group properties are supported:

* `id`
* `displayName`
* `mail`

#### Fields – Additional

If you need extra fields that aren’t included in the [default list](/data-imports-guide/cloud-users/entraid#about-default-fields), you can enter them here. This is useful for pulling in custom or extension attributes.

#### Action

Choose what Hornbill should do with the users it finds.  This is a mandatory field that must be set.

* **Create**: Only adds new users who don’t already exist in Hornbill.  
* **Update**: Only updates users who already exist in Hornbill.  
* **Create & Update**: Adds new users *and* updates existing ones.

#### Account Status

Set the status of a new or existing user.  These are mandatory fields that must be set.

* **Action**: Choose when this status should be applied.
  * **Create**: Only adds new users who don’t already exist in Hornbill.  
  * **Create & Update**: Adds new users *and* updates existing ones.
* **User Status**: Set the status of the user to *Active*, *Suspended*, or *Archived*.

#### User Properties

Define how user data should map to Hornbill fields during import.

* Supports [Mustache templates](/data-imports-guide/cloud-users/data-mapping) for custom formatting or combining data.
* Some input fields offer auto-complete to help you select fields from your Entra ID source. If you know the name of the field you can start typing it, otherwise if you start by typing `{` all available fields will be displayed for you to select from.
* The **User Type**, **User Type Action**, **User ID**, and **Handle** fields are mandatory and must be set before you can save the Source/Import Options.

***About the Action and User Type Action import options.*** When you are configuring your import in the **Data Source** tab, it's possible to configure the import with some inconsistent options. These won't cause the import to fail, but it's important to understand the relationship between some of the options available, as sometimes, in order for one option to be effective, a related option must also be configured in a particular way.

* The Action option is the action to perform for the users returned from your data source. (This is the main Action option). You can choose **Create**, **Update**, or **Create & Update**.

    ![The action to perform](/_books/data-imports-guide/cloud-users/images/main-action-for-import.png)

* In the User Properties option, there are account details you must specify, including User Type Action. This specifies when an update to the user type should be performed. The options are **Create**, and **Create & Update**.

    ![Update the user type](/_books/data-imports-guide/cloud-users/images/user-type-action.png)

    This User Type Action is *only* relevant when the main action is **Create** or **Create & Update**. When the main action is **Update**, there are no user types to create. While you must select an option,  it has no bearing on the import.

##### Manager mapping and advanced queries

When you map the **Manager** field in your import configuration, Hornbill automatically requests manager details from the Microsoft Graph API alongside each user record. This is the only way to reliably retrieve manager information without making a separate API call for every user being imported.

However, this has an important consequence: **Microsoft Graph does not support [advanced queries](https://learn.microsoft.com/en-us/graph/aad-advanced-queries?tabs=http) when manager data is being requested at the same time.** Advanced queries unlock additional filter operators and the ability to filter on a wider range of properties.

This means:

* **If you do not map the Manager field** – advanced queries are fully supported, and you can use the extended filter operators and properties described in the [advanced queries documentation](https://learn.microsoft.com/en-us/graph/aad-advanced-queries?tabs=http).
* **If you do map the Manager field** – Hornbill automatically includes the manager expand in the API request, and advanced queries cannot be used. Your filter must use only the [standard OData operators and filterable properties](https://learn.microsoft.com/en-us/graph/filter-query-parameter?tabs=http).

If your filtering requirements need advanced query support, consider whether manager data is essential for your import. If it is not, leave the Manager field unmapped to take full advantage of advanced filtering.

#### Memberships

### About memberships and role assignments

When you are configuring your import in the **Data Source** tab, there are two essential import options you need to be aware of: *group memberships* and *role assignments*.

A user can be assigned a membership to an organizational group. An organizational group in Hornbill can be a *Department*, *Company*, or *Team*. The user's membership to a group can be set as *Member*, *Team Leader*, or *Manager*.

#### Profile Picture

Choose what Hornbill should do with the users' profile pictures it finds:

* **No Processing** – Ignore the image data.
* **Update** – Take the image data and make it the user's profile picture in Hornbill.  
* **Clear** – Use this option to remove the image data in Hornbill during the import.

> **Note**: The Clear option in Profile Picture is only relevant when the main action is **Update** or **Create & Update**.

---

## About default fields

When you connect to Entra ID to import users, the system brings in a standard set of information (called *fields*) for each person. These fields are available for mapping and filtering. Here's what gets included by default:

* **accountEnabled** – Is the account active?
* **businessPhones** – A comma-separated list of the business phone numbers stored against the user.
* **city** – City location.
* **companyName** – Name of the company.
* **country** – Country location.
* **createdDateTime** – When the user was created.
* **department** – Department name.
* **displayName** – Full name.
* **employeeHireDate** – Date they were hired.
* **employeeId** – Internal employee ID.
* **employeeType** – Type of employee (e.g., full-time, contractor).
* **givenName** – First name.
* **id** – Unique ID (system-generated).
* **jobTitle** – Job title or role.
* **mail** – Email address.
* **mailNickname** – Short name used in email.
* **manager.id** – The ID of the user's manager.
* **manager.displayName** – The display name of the user's manager.
* **manager.mail** – The email address of the user's manager.
* **manager.userPrincipalName** – The login email or unique sign-in name of a user's assigned manager.
* **memberOf** – An array of groups that the user belongs to. See [memberOf field mapping](/data-imports-guide/cloud-users/entraid#advanced-field-mapping-with-the-memberof-property) for more information.
* **memberOfNames** – A comma-separated list of the display names of the groups that the user belongs to.
* **mobilePhone** – Mobile number.
* **officeLocation** – Office address or location.
* **onPremisesDistinguishedName** – Internal system path.
* **onPremisesDomainName** – Local network domain name.
* **onPremisesSamAccountName** – Network username.
* **onPremisesUserPrincipalName** – Network login name (e.g. email).
* **otherMails** – A comma-separated list of extra email addresses stored against the user.
* **postalCode** – ZIP/postal code.
* **securityIdentifier** – Security ID from the system.
* **state** – State or region.
* **streetAddress** – Street address.
* **surname** – Last name.
* **userPrincipalName** – Main login name (often their email).
* **userType** – Type of user (e.g., member or guest).

## Advanced field mapping with the memberOf property

::: warning
This section describes an advanced feature. Before using these examples, ensure you are familiar with:

* [Mustache templating](/data-imports-guide/cloud-users/data-mapping) (for building dynamic mappings)  
* [Microsoft Graph User resource](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties) (to understand available fields and their structure)  

A solid understanding of both is strongly recommended, as incorrect mappings may result in unexpected data imports.
:::

When importing users from Entra ID (via the Microsoft Graph API), each user includes a `memberOf` property. This property contains an array of group objects that the user belongs to.

Each group object provides the following fields:

| Field               | Type             | Example Value                          | Description                                                |
| ------------------- | ---------------- | -------------------------------------- | ---------------------------------------------------------- |
| **id**              | `string (GUID)`  | `a1b2c3d4-e5f6-7890-1234-56789abcdef0` | Unique identifier for the group in Entra ID                |
| **displayName**     | `string`         | `HR Team`                              | Human-readable name of the group                           |
| **mailEnabled**     | `boolean`        | `true`                                 | Indicates if the group is mail-enabled                     |
| **securityEnabled** | `boolean`        | `false`                                | Indicates if the group is security-enabled                 |
| **uniqueName**      | `string`         | `contoso.com/Groups/HR`                | Unique path-like name of the group                         |
| **visibility**      | `string`         | `Private`                              | Group visibility (`Public`, `Private`, `HiddenMembership`) |
| **mail**            | `string (email)` | `hr-team@contoso.com`                  | Group’s email address, if available                        |

### Accessing memberOf data in templates

Since `memberOf` is an array, you can access its values using Mustache template iteration. For example:

```cmd
{{#memberOf}}
  Group ID: {{id}}
  Group Name: {{displayName}}
  Group Email: {{mail}}
{{/memberOf}}
```

This will output details for every group the user is a member of.

### Mapping examples

---

#### Example 1: Mapping a single group name

If you want the first group name:

```cmd
{{memberOf.0.displayName}}

```

---

#### Example 2: Mapping all group names as a comma-separated list

```cmd
{{#memberOf}}{{displayName}},{{/memberOf}}

```

Example output:

> ```text
> HR Team,Finance,IT,
> ```

---

#### Example 3: Filtering by group type

Map only mail-enabled groups:

```cmd
{{#memberOf}}
 {{#mailEnabled}}{{displayName}}{{/mailEnabled}}
{{/memberOf}}
```

---

#### Example 4: Mapping all group IDs

```cmd
{{#memberOf}}{{id}};{{/memberOf}}

```

---

### Best practices for mapping using memberOf

* Decide if your field should contain one value (e.g. the first group) or multiple values (e.g. a list).
* Use iteration (`{{#memberOf}}...{{/memberOf}}`) for handling multiple groups.
* Be aware of trailing separators (commas, semicolons) when concatenating multiple values.
* Some fields (like `mailEnabled` and `securityEnabled`) are boolean flags — useful for filtering, not direct mapping.

With these patterns, you can flexibly map group membership information from Entra ID into your Hornbill user fields.
