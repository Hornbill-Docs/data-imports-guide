# Authorization

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
- **Asset Management Usert** - Allows the utility to create and update Asset Management records in Service Manager.
- **Hornbill Service Manager Integrations** - Enables a number of entity and stored query privileges. ***NOTE*** - This role is only intended for accounts that are used for integrations or to perform data imports, and should not be applied to interactive user accounts. 

## API Key Rules
This utility uses ([API keys](/esp-fundamentals/security/api-keys)):
```cmd
activity:postMessage
bpm:processSpawn2
session:getApplicationOption
admin:userGetInfo
data:entityAddRecord
data:entityAttachFile
data:entityBrowseRecords2
data:entityUpdateRecord
data:profileCodeLookup
data:queryExec
system:logMessage
task:taskComplete
task:taskCreate2
apps/com.hornbill.servicemanager/Requests:holdRequest
```

## KeySafe

For the import utility to access data from your source database, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

Once the relevant key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.
