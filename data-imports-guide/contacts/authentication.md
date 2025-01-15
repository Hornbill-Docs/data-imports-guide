# Authentication

The Contact Import utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store credentials for the contact data source.

## API Keys

For the utility to read, create and update records via the Hornbill API, it requires an [API Key](/esp-fundamentals/security/api-keys) to be securely stored alongside the client.

### User

Every action within Hornbill must be performed in the context of a user account. The user account must possess roles for the platform and applications that you are granting access to via the import utility. The above comment about roles refers to the [Hornbill Security Model](/esp-fundamentals/security/account-types) when associating roles with user accounts. This security measure prevents you from inflating your session rights or granting a user more rights than you have yourself.

:::important
We strongly recommend that you create a Service Account in your Hornbill instance, and API Keys against that account which can then be used to perform the required API calls back into Hornbill. 

Please read the [API Key documentation](/esp-fundamentals/security/api-keys) and [best practice guide](/esp-fundamentals/best-practice/platform-api-keys) before creating API keys against your user records.
:::

The service account that you create must be of type `User` (not `Basic`), and be granted the following roles:

- **User Role** - Allows the utility to perform entity actions in the Hornbill platform.
- **Contact Admin** - Allows the utility to create and update Contact records in Service Manager.
- **Hornbill Service Manager Integrations** - Grants several privileges for access to entities and the execution of stored queries. ***NOTE*** - This role is only intended for accounts that are used for integrations or to perform data imports, and should not be applied to interactive user accounts. 

### API Key Rules

The Contact Imports require access to the following Hornbill Platform and application APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

```cmd
admin:keysafeGetKey
admin:portalSetContactAccess
data:entityAddRecord
data:entityBrowseRecords2
data:entityUpdateRecord
system:logMessage
apps/com.hornbill.core/Contact:changeOrg
apps/com.hornbill.servicemanager/ContactOrgRequests:changeOrgRequestSetting
apps/com.hornbill.servicemanager/ServiceSubscriptions:add
```

:::tip
The `csv - CSV / Text file(s)` data source reads its data from the file system (local or network), and therefore does not require Keysafe keys.
:::
