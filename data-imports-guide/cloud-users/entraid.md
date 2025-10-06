---
title: Setting up your Entra ID user import
layout: article
keywords: copy import 
---

# Setting up your Entra ID user import

Before you begin, make sure you [understand the ways of filtering the data you import](/data-imports-guide/cloud-users/entraid#filtering-import-only-the-users-you-need).

This article also covers the [default fields](/data-imports-guide/cloud-users/entraid#about-default-fields) Hornbill brings in for each user when you do an import.

For admins familiar with Mustache templating and Microsoft Graph User resource, this article also provides information about an advanced feature: [mapping fields using the memberOf property](/data-imports-guide/cloud-users/entraid#advanced-field-mapping-with-the-memberof-property).

## Creating an import configuration

**To find Cloud Data Imports:**

Navigate to **Configuration > Platform Configuration > Data > Cloud Data Imports**.

**To create an import configuration:**

1. At the top right of the configurations list, click **+ Add New**.
    ::: note
    If this is your first import configuration, click the button that says **No import configurations are set up. Click here to create your first import.**
    :::
2. Give your import a clear and unique name.
3. Click **Create**.
4. (Optional) Add a description in the **Description** field to explain what the import does.

**To choose the data source:**

1. Click the **Data Source** tab.
2. In the **Data Source Settings** area, in the **Import** field, click the edit icon.
3. In the Hornbill Integration Bridge dialog, select **Cloud Data Imports** > **Users** > **Entra ID**.
4. Click **Apply**.

    The **Source / Import Options** section appears, with settings you can customize.

**To customize the settings in the Data Source tab:**
1. Use the edit buttons to make your choices.
1. Make sure to read [the information about source/import options](/data-imports-guide/cloud-users/entraid#sourceimport-options) as you configure your import.

### Source/import options

This section covers the options available when setting up your Entra ID user import. These settings give you control over what gets imported, when, and how.

::: important
### About memberships and role assignments
When you are configuring your import in the **Data Source** tab, there are two essential import options you need to be aware of: *memberships* and *role assignments*.

A user record is useful only if it has been assigned to a membership. A membership is an organizational grouping in Hornbill, such as *Department* or *Company*. The record can be assigned to an organizational grouping as *Member*, *Team Leader*, or *Manager*. You can create any number of memberships to apply to the users you are importing.

For important details, refer to [Memberships and roles](/data-imports-guide/cloud-users/memberships-and-roles) as you are assigning memberships and roles in your import configuration.
:::

<!--Role assignments take effect when we create a user for the first time, but if we update an existing user, it won’t have any effect.-->

---

#### KeySafe Key

Choose the **KeySafe Key** [you created earlier](/data-imports-guide/cloud-users/overview#setting-up-a-keysafe-key). This is what allows Hornbill to securely access your Entra ID account.

---

#### Query (Optional)

This is where you can enter a filter to limit which users are imported. Make sure you [understand the ways of filtering the data you import](/data-imports-guide/cloud-users/entraid#filtering-import-only-the-users-you-need). 

::: caution
If you leave this field empty, and do not provide any Group ID filters, then *all users* from your Entra ID account will be processed.
:::

- Hornbill uses Microsoft’s [Graph API](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0&tabs=http) to access Entra ID.
- You can write a query using OData to select specific users based on things like department, email domain, etc.

Resources to help you write queries:
- [$filter syntax guide](https://learn.microsoft.com/en-us/graph/filter-query-parameter?tabs=http)
- [List of available user fields](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties)


::: important
Not all properties from the [Microsoft Graph User Resource type](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0) can be used in query filters, and a number of these properties do not support the full range of OData operators to query against. See the [list of available user fields](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties) for a full list of properties, which details which of the properties can be used in the query filters, and which OData operators are supported by each.

For example:

- The value of `onPremisesDistinguishedName` can be returned and mapped into your Hornbill user records, but the field *cannot* be used in a query filter.
- The value of `onPremisesUserPrincipalName` can be returned and mapped into your Hornbill user records, and *does* support filtering using the documented OData query operators.
:::

---

#### Group ID filter

This is where you can provide additional Group IDs, to further filter the list of users returned by your optional Query (as detailed above).

This is a list of group identifiers, and if each user returned by the query is a member of at least one of the groups, then that user will be processed by the data import job. The following group properties are supported:

- `id`
- `displayName`
- `mail`

#### Fields – Additional

If you need extra fields that aren’t included in the [default list](/data-imports-guide/cloud-users/entraid#about-default-fields), you can enter them here. This is useful for pulling in custom or extension attributes.

---

#### Action

Choose what Hornbill should do with the users it finds:

- **Create** – Only adds new users who don’t already exist in Hornbill.  
- **Update** – Only updates users who already exist in Hornbill.  
- **Create & Update** – Adds new users *and* updates existing ones.

---

#### Account Status

Set the status of the users being imported (e.g. *Active*, *Inactive*), and choose when this status should be applied.

---

#### User Properties

Define how user data should map to Hornbill fields during import.

- Supports [Mustache templates](/data-imports-guide/cloud-users/data-mapping) for custom formatting or combining data.
- Input fields offer auto-complete to help you select fields from your Entra ID source.

---

## Filtering: Import only the users you need

You don’t have to import every user record. You can choose which to bring in by setting up simple filters (also called *queries*).

Here are a few examples:

---

### Filter by department

To import only users from the Application Development department:

```cmd
department eq 'Application Development'

```

---

### Filter by email domain

To import users whose email address ends with @hornbill.com:

```cmd
endswith(mail,'@hornbill.com')

```

---

### Combine filters

You can also combine filters. For example, to bring in users who both:

- Have an email address ending in @hornbill.com and
- Work in the Application Development department:

```cmd
endswith(mail,'@hornbill.com') and department eq 'Application Development'

```

## About default fields

When you connect to Entra ID to import users, the system brings in a standard set of information (called *fields*) for each person. These fields are available for mapping and filtering. Here's what gets included by default:

- **accountEnabled** – Is the account active?
- **businessPhones** – A comma-separated list of the business phone numbers stored against the user
- **city** – City location
- **companyName** – Name of the company
- **country** – Country location
- **createdDateTime** – When the user was created
- **department** – Department name
- **displayName** – Full name
- **employeeHireDate** – Date they were hired
- **employeeId** – Internal employee ID
- **employeeType** – Type of employee (e.g., full-time, contractor)
- **givenName** – First name
- **id** – Unique ID (system-generated)
- **jobTitle** – Job title or role
- **mail** – Email address
- **mailNickname** – Short name used in email
- **manager.id** - The ID of the user's manager
- **manager.displayName** - The display name of the user's manager
- **manager.mail** - The email address of the user's manager
- **manager.userPrincipalName** - 
- **memberOf** – An array of groups that the user belongs to. See [memberOf field mapping](/data-imports-guide/cloud-users/entraid#advanced-field-mapping-with-the-memberof-property) for more information.
- **memberOfNames** – A comma-separated list of the display names of the groups that the user belongs to.
- **mobilePhone** – Mobile number
- **officeLocation** – Office address or location
- **onPremisesDistinguishedName** – Internal system path
- **onPremisesDomainName** – Local network domain name
- **onPremisesSamAccountName** – Network username
- **onPremisesUserPrincipalName** – Network login name (e.g. email)
- **otherMails** – A comma-separated list of extra email addresses stored against the user
- **postalCode** – ZIP/postal code
- **securityIdentifier** – Security ID from the system
- **state** – State or region
- **streetAddress** – Street address
- **surname** – Last name
- **userPrincipalName** – Main login name (often their email)
- **userType** – Type of user (e.g., member or guest)


## Advanced field mapping with the memberOf property

::: warning
This section describes an advanced feature. Before using these examples, ensure you are familiar with:

- [Mustache templating](/data-imports-guide/cloud-users/data-mapping) (for building dynamic mappings)  
- [Microsoft Graph User resource](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties) (to understand available fields and their structure)  

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

**Example 1: Mapping a single group name**

If you want the first group name:

```cmd
{{memberOf.0.displayName}}

```

--- 
**Example 2: Mapping all group names as a comma-separated list**

```cmd
{{#memberOf}}{{displayName}},{{/memberOf}}

```

Example output:

> ```text
> HR Team,Finance,IT,
> ```

---

**Example 3: Filtering by group type**

Map only mail-enabled groups:

```cmd
{{#memberOf}}
 {{#mailEnabled}}{{displayName}}{{/mailEnabled}}
{{/memberOf}}
```

---

**Example 4: Mapping all group IDs**

```cmd
{{#memberOf}}{{id}};{{/memberOf}}

```

---

### Best practices for mapping using memberOf

* Decide if your field should contain one value (e.g. the first group) or multiple values (e.g. a list).
* Use iteration (`{{#memberOf}}...{{/memberOf}}`) for handling multiple groups.
* Be aware of trailing separators (commas, semicolons) when concatenating multiple values.
* Some fields (like `mailEnabled` and `securityEnabled`) are boolean flags —- useful for filtering, not direct mapping.

With these patterns, you can flexibly map group membership information from Entra ID into your Hornbill user fields.
