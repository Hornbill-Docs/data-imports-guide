# Authentication

The User Import - Database utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store credentials for the source database.

## API Keys

[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/users_apikeys.md]]

### API Key Rules

The User Imports require access to the following Hornbill Platform APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

```cmd
activity:profileImageSet 
admin:keysafeGetKey 
admin:sysOptionGet 
admin:userAddGroup 
admin:userAddRole 
admin:userCreate 
admin:userDeleteGroup 
admin:userProfileSet 
admin:userSetAccountStatus 
admin:userUpdate 
data:entityAddRecord 
data:entityGetRecord 
data:entityUpdateRecord 
data:queryExec 
session:getSystemLicenseInfo 
```

## KeySafe

For the import utility to access data from your source database, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

### Key Creation

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`;
* Click `+ Create New Key`
* Choose a key type of `Database Authentication`
* Give the KeySafe key a Title
* Optionally add a Description
* Populate the following fields on the form:
  * `Server` - The IP address or hostname of your database host.
  * `Port` - The port used to connect to your database.
  * `Database` - The database name/ID.
  * `Username` - The username of the account that should be used to authenticate the connection to your database.
  * `Password` - The password for the above account. 
* Click `Create Key`

Once the key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

### Key Example

<img src="/_books/data-imports-guide/users/database/images/db_user_import_keysafe.jpg" width="650px" alt="KeySafe Example"/>