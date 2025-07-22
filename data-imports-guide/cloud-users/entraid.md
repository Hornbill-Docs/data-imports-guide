# Setting Up Your First Entra ID User Import

Before you can start importing user data from Entra ID, there are a few setup steps to follow.

---

## Step 1: Set Up a Keysafe Key

To securely connect Hornbill to your Entra ID, you’ll first need to create something called a **Keysafe Key**.

This key safely stores your login credentials and gives Hornbill permission to access your Entra ID user data.

- Use the **Entra ID User Import** Key type.
- For help creating the key, follow the [Keysafe setup guide](/esp-config/security/keysafe) in the Hornbill Platform Configuration Guide.

Once your Keysafe Key is ready and connected to your Entra ID account, you’re all set to move on.

---

## Step 2: Create Your First Import

**To find Cloud Data Imports:**

Navigate to **Configuration > Platform Configuration > Data > Cloud Data Imports**.

---

**To create a new import configuration:**

1. At the top right of the configurations list, click **+ Add New**.
    ::: note
    If this is your first import configuration, click the button that says **No import configurations are set up. Click here to create your first import.**
    :::
2. Give your import a clear and unique name.
3. Click **Create**.
4. (Optional) Add a description in the **Description** field to explain what the import does.

---

**To choose the data source:**

1. Click the **Data Source** tab.
2. In the **Data Source Settings** area, in the **Import** field, click the edit icon.
3. In the Hornbill Integration Bridge dialog, select **Cloud Data Imports** > **Users** > **Entra ID**.
4. Click **Apply**.

Once you've done that, the **Source / Import Options** section appears, with settings you can customize.

### Source / Import Options

Here’s a breakdown of the options available when setting up your Entra ID user import. These settings give you control over what gets imported, when, and how.

---

#### Keysafe Key

Choose the **Keysafe Key** you created earlier — this is what allows Hornbill to securely access your Entra ID account.

[Learn how to create a Keysafe Key →](#step-1-set-up-a-keysafe-key)

---

#### Query (Optional)

This is where you can enter a **filter** to limit which users are imported.

- Hornbill uses Microsoft’s [Graph API](https://learn.microsoft.com/en-us/graph/api/user-list?view=graph-rest-1.0&tabs=http) to access Entra ID.
- You can write a query using oData to select specific users based on things like department, email domain, etc.

Resources to help you write queries:
- [$filter syntax guide](https://learn.microsoft.com/en-us/graph/filter-query-parameter?tabs=http)
- [List of available user fields](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties)

::: important
If you leave this blank, *all users* from your Entra ID account will be imported.
:::

---

#### Fields – Additional

If you need extra fields that aren’t included in the [default list](#default-fields), you can enter them here. This is useful for pulling in custom or extension attributes.

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
- Input fields offer **auto-complete** to help you select fields from your Entra ID source.

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

## Filtering: Only Bring In Who You Need

You don’t have to import *everyone*. You can choose who to bring in by setting up simple filters (also called "queries").

Here are a few examples:

### Filter by Department

To import only users from the Application Development department:

> ```text
> department eq 'Application Development'
> ```

### Filter by Email Domain

To import users whose email address ends with @hornbill.com:

> ```text
> endswith(mail,'@hornbill.com')
> ```

### Combine Filters

You can also combine filters. For example, to bring in users who both:

- Have an email address ending in @hornbill.com and
- Work in the Application Development department:

> ```text
> endswith(mail,'@hornbill.com') and department eq 'Application Development'
> ```

---

## Default Fields

When you connect to **Entra ID** to import users, the system brings in a standard set of information (called "fields") for each person. These fields are available for mapping and filtering. Here's what gets included by default:

- **accountEnabled** – Is the account active?
- **businessPhones** – Work phone number(s)
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
- **manager** – Their manager
- **memberOf** – Groups the user belongs to
- **memberOfNames** – A comma-separated list of the Display Names of the Groups that the user belongs to
- **mobilePhone** – Mobile number
- **officeLocation** – Office address or location
- **onPremisesDistinguishedName** – Internal system path
- **onPremisesDomainName** – Local network domain name
- **onPremisesSamAccountName** – Network username
- **onPremisesUserPrincipalName** – Network login name (like email)
- **otherMails** – Any extra email addresses
- **postalCode** – ZIP/postal code
- **securityIdentifier** – Security ID from the system
- **state** – State or region
- **streetAddress** – Street address
- **surname** – Last name
- **userPrincipalName** – Main login name (often their email)
- **userType** – Type of user (e.g., member or guest)
