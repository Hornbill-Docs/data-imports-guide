# Setting up your Google Workspace user import

Before you begin, make sure you [understand the ways of filtering the data you import](/data-imports-guide/cloud-users/googleworkspace#filtering-import-only-the-users-you-need).

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
3. In the Hornbill Integration Bridge dialog, select **Cloud Data Imports** > **Users** > **Google Workspace**.
4. Click **Apply**.

    The **Source / Import Options** section appears, with settings you can customize.

**To customize the settings in the Data Source tab:**
1. Use the edit buttons to make your choices.
1. Make sure to read [the information about source/import options](/data-imports-guide/cloud-users/googleworkspace#sourceimport-options) as you configure your import.

### Source/import options

This section covers the options available when setting up your Google Workspace user import. These settings give you control over what gets imported, when, and how.

::: important
### About memberships and role assignments
When you are configuring your import in the **Data Source** tab, there are two essential import options you need to be aware of: *memberships* and *role assignments*.

A user record is useful only if it has been assigned to a membership. A membership is an organizational grouping in Hornbill, such as *Department* or *Company*. The record can be assigned to an organizational grouping as *Member*, *Team Leader*, or *Manager*. You can create any number of memberships to apply to the users you are importing.

For important details, refer to [Memberships and roles](/data-imports-guide/cloud-users/memberships-and-roles) as you are assigning memberships and roles in your import configuration.
:::

---

#### Keysafe Key

Choose the **KeySafe Key** [you created earlier](/data-imports-guide/cloud-users/overview#setting-up-a-keysafe-key). This is what allows Hornbill to securely access your Google Workspace account.

---

#### **Customer ID** (Optional)  
 
Enter your Google Workspace Customer ID. If you leave this blank, `my_customer` will be used by default.

---

#### **Domain** (Optional)  

Specify your primary domain to import users from, if you want to limit the import.

---

#### **Query** (Optional)  

Use this to search or filter users based on different profile fields, including custom fields.  
See [Google’s Search Users documentation](https://developers.google.com/workspace/admin/directory/v1/guides/search-users) for examples.

::: important
If no query is provided, all users from your Google Workspace domain will be imported!
:::

---

#### **Return Deleted User**  

Choose whether to include deleted users in the import.

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
- Input fields offer auto-complete to help you select fields from your Google Workspace source.

***About the Action and User Type Action import options.*** When you are configuring your import in the **Data Source** tab, it's possible to configure the import with some inconsistent options. These won't cause the import to fail, but it's important to understand the relationship between some of the options available, as sometimes, in order for one option to be effective, a related option must also be configured in a particular way.

* The Action option is the action to perform for the users returned from your data source. (This is the main Action option). You can choose **Create**, **Update**, or **Create & Update**.

    ![The action to perform](/_books/data-imports-guide/cloud-users/images/main-action-for-import.png)

* In the User Properties option, there are account details you must specify, including User Type Action. This specifies when an update to the user type should be performed. The options are **Create**, and **Create & Update**.

    ![Update the user type](/_books/data-imports-guide/cloud-users/images/user-type-action.png)

    This User Type Action is *only* relevant when the main action is **Create** or **Create & Update**. When the main action is **Update**, there are no user types to create. While you must select an option,  it has no bearing on the import.

#### Profile Picture
Choose what Hornbill should do with the users' profile pictures it finds:

- **No Processing** – Ignore the image data.
- **Update** – Take the image data and make it the user's profile picture in Hornbill.  
- **Clear** – Use this option to remove the image data during the import.

The Clear option in Profile Picture is only relevant when the main action is **Update** or **Create & Update**.

---

## Filtering: Import only the users you need

You don’t have to bring in every user record. Use queries to filter users by department, location, or other fields.

Check out [Google’s example queries](https://developers.google.com/workspace/admin/directory/v1/guides/search-users#examples) for ideas.

---

## Available fields

When you connect to Google Workspace to import users, Google presents you with a standard set of fields for each person. These fields are available for mapping and filtering. Here's what gets included:

### Name
 - `displayName`
 - `familyName`
 - `fullName`
 - `givenName`

### Account Information
 - `agreedToTerms`
 - `aliases`
 - `archived`
 - `changePasswordAtNextLogin`
 - `creationTime`
 - `customSchemas`
 - `deletionTime`
 - `etag`
 - `id`
 - `includeInGlobalAddressList`
 - `ipWhitelisted`
 - `isAdmin`
 - `isDelegatedAdmin`
 - `isEnforcedIn2Sv`
 - `isEnrolledIn2Sv`
 - `isMailboxSetup`
 - `keywords`
 - `kind`
 - `languages`
 - `lastLoginTime`
 - `notes`
 - `orgUnitPath`
 - `suspended`
 - `suspensionReason`
 - `thumbnailPhotoEtag`
 - `thumbnailPhotoUrl`

### External ID Fields
 - `accountExternalId`
 - `customerExternalId`
 - `loginExternalId`
 - `networkExternalId`
 - `organizationExternalId`

### Contact Information
 - `companyMainPhone`
 - `homeEmail`
 - `homePhone`
 - `mobilePhone`
 - `otherEmail`
 - `primaryEmail`
 - `recoveryEmail`
 - `recoveryPhone`
 - `workEmail`
 - `workMobilePhone`
 - `workPhone`

### Organization Information
 - `dottedLineManager`
 - `manager`

#### Organization Types
  - `domainOnlyOrganization`
  - `primaryOrganization`
  - `schoolOrganization`
  - `unknownOrganization`
  - `workOrganization`

The following fields are available, where populated, for each of the above organization types:
 - `costCenter`
 - `department`
 - `description`
 - `domain`
 - `fullTimeEquivalent`
 - `location`
 - `name`
 - `symbol`
 - `title`

 And can be mapped in the following way: `{{domainOnlyOrganization.costCenter}}`.

 ### Address Information

 #### Address Types
 - `homeAddress`
 - `otherAddress`
 - `primaryAddress`
 - `workAddress`

 The following fields are available, where populated, for each of the above organization types:
 - `country`
 - `countryCode`
 - `extendedAddress`
 - `formatted`
 - `locality`
 - `poBox`
 - `postalCode`
 - `postalCode`
 - `region`
 - `region`
 - `streetAddress`

 And can be mapped in the following way: `{{homeAddress.streetAddress}}`.
 
 ### Location Information

#### Location Types
- `defaultLocation`
- `deskLocation`

 The following fields are available, where populated, for each of the above organization types:
 - `area`
 - `buildingId`
 - `deskCode`
 - `floorName`
 - `floorSection`

 And can be mapped in the following way: `{{defaultLocation.buildingId}}`.