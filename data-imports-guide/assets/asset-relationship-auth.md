# Authentication

The Asset Relationship Import utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store credentials for the asset data source.

## API Keys

For the utility to read, create and update records via the Hornbill API, it requires an [API Key](/esp-fundamentals/security/api-keys) to be securely stored alongside the client.

### User

Every action within Hornbill must be performed in the context of a user account. The user account must possess roles for the platform and applications that you are granting access to via the import utility. The above comment about roles refers to the [Hornbill Security Model](/esp-fundamentals/security/account-types) when associating roles with user accounts. This security  measure prevents you from inflating your session rights, or granting a user more rights than you have yourself.

:::important
We strongly recommend that you create a Service Account in your Hornbill instance, and API Keys against that account which can then be used to perform the required API calls back into Hornbill. 

Please read the [API Key documentation](/esp-fundamentals/security/api-keys) and [best practice guide](/esp-fundamentals/best-practice/platform-api-keys) before creating API keys against your user records.
:::

The service account that you create must be of type `User` (not `Basic`), and be granted the following roles:

- **User Role** - Allows the utility to perform entity actions in the Hornbill platform.
- **Asset Management User** - Allows the utility to create and update Asset Management records in Service Manager.
- **Hornbill Service Manager Integrations** - Enables a number of entity and stored query privileges. ***NOTE*** - This role is only intended for accounts that are used for integrations or to perform data imports, and should not be applied to interactive user accounts. 

### API Key Rules

The Asset Relationship Imports require access to the following Hornbill Platform and application APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

This utility uses ([API keys](https://docs.hornbill.com/esp-fundamentals/security/api-keys)):

```cmd
data:entityAddRecord
data:entityDeleteRecord
data:entityUpdateRecord
data:getRecordCount
data:queryExec
system:logMessage
apps/com.hornbill.servicemanager/Asset:linkAsset
apps/com.hornbill.servicemanager/Asset:unlinkAsset
```
## KeySafe

For the import utility to access data from your source database, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

Once the relevant key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

### Key Types

As the Asset Relationship Import utility supports the import of asset data from many different data sources as listed below for the following key type:

* [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication) - Used for the following data sources:
  * `mssql` - Microsoft SQL Server (2005 or above).
  * `mysql` - MySQL 4.1 or above, or any version of MariaDB.
  * `mysql320` - MySQL Server v3.2.0 to v4.0.
  * `odbc` - ODBC driver.
  * `swsql` - Supportworks SQL (Core Services v3.x).
