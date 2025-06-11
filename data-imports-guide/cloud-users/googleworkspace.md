# Setting Up Your First Google Workspace User Import

Before you can start importing user data from Google Workspace, there are a few setup steps to follow.

---

## Step 1: Set Up a Keysafe Key

To securely connect Hornbill to your Google Workspace, you’ll first need to create something called a **Keysafe Key**.

This key safely stores your login credentials and gives Hornbill permission to access your Google Workspace user data.

- Use the **Google Workspace User Management** Key type.
- For help creating the key, follow the [Keysafe setup guide](/esp-config/security/keysafe) in the Hornbill Platform Configuration Guide.

Once your Keysafe Key is ready and connected to your Google Workspace account, you’re all set to move on.

---

## Step 2: Create Your First Import

### Where to Find Cloud Data Imports

To get started:

1. Go to `Configuration`
2. Select `Platform Configuration`
3. Navigate to:  
   `Data > Cloud Data Imports`

---

### Create a New Import Configuration

1. Click the `+ Add New` button at the top-right.
2. Give your import a clear and unique name.
3. Click `Create`.
4. (Optional) Add a description in the **Description** field to explain what the import does.

---

### Choose the Data Source

1. Click on the **Data Source** tab.
2. In the **Data Source Settings** area, click the edit icon next to the **Import** field.
3. In the pop-up window, select the following:
   - `Cloud Data Imports`
   - `Users`
   - `Google Workspace`
   - Then click `Apply`

Once you've done that, the **Source / Import Options** section will appear with settings you can customize.

---

### Source / Import Options

Here’s what you can set up:

---

#### Keysafe Key

Choose the **Keysafe Key** you created earlier — this is what allows Hornbill to securely access your Google Workspace account.

[Learn how to create a Keysafe Key →](#step-1-set-up-a-keysafe-key)

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
If no query is provided, *all users* from your Google Workspace domain will be imported!
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

Set the **status** of the users being imported (e.g. active, inactive), and choose *when* this status should be applied.

---

#### User Properties

Define how user data should map to Hornbill fields during import.

- Supports [Mustache templates](/data-imports-guide/cloud-users/data-mapping) for custom formatting or combining data.
- Input fields offer **auto-complete** to help you select fields from your Google Workspace source.

---

### Memberships

This section lets you assign imported users to **organizational groups** in Hornbill.

- **Action** – When should the user be added to the group?
  - `Create` – Add user only during Create actions
  - `Update` – Add user only during Update actions
  - `Create & Update` – Add user during both
  - `Unassign if Assigned` – Remove user from the group if already assigned
- **Organization ID** – The ID of the group you want to assign
- **Membership** – Choose one:
  - `Member`
  - `Team Leader`
  - `Manager`
- **Can View Tasks** – Should this user be able to *see* tasks for the group?
- **Can Action Tasks** – Should this user be able to *work on* tasks?
- **Single Assignment per Group Type** – Ensures the user belongs to just one group per type, removing them from others of the same type.

---

### Role Assignments

Assign roles to users as part of the import process.

- **Action** – When should the role be assigned or removed?
  - `Create` – Assign role only when the user is newly created
  - `Update` – Assign role only during updates
  - `Create & Update` – Assign during both
  - `Unassign if Assigned` – Remove role if the user already has it
- **Role** – The name of the role to assign

---

## Filtering: Import Only the Users You Need

You don’t have to bring everyone in! Use **queries** to filter users by department, location, or other fields.

Check out [Google’s example queries](https://developers.google.com/workspace/admin/directory/v1/guides/search-users#examples) for ideas.

---

## Available Fields

When you connect to **Google Workspace** to import users, Google presents us with a standard set of fields for each person. These fields are available for mapping and filtering. Here's what gets included:

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