# Authentication

The User Import - LDAP utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store LDAP credentials.

## API Keys

For the utility to read, create and update records via the Hornbill API, it requires an API key to be securely stored alongside the client.

:::note
We recommend that you read the [API Key documentation](/esp-fundamentals/security/api-keys) and [best practice guide](/esp-fundamentals/best-practice/platform-api-keys) before creating API keys against your user records.
:::

### User

Every action within Hornbill must be performed in the context of a user account. As well as the chosen user account possessing the `User Import` role (see below) which facilitates the creation of the user accounts into the Hornbill platform, and the updating of user properties, the user account must possess roles for the applications that you are granting access to via the import utility. The above comment about roles refers to the [Hornbill Security Model](/esp-fundamentals/security/account-types) when associating roles with user accounts. This security  measure prevents you from inflating your session rights, or granting a user more rights than you have yourself.

:::important
We strongly recommend that you create a Service Account in your Hornbill instance, which can be used to perform the required API calls back into Hornbill.
:::

The service account that you create must be of type `User` (not `Basic`), and be granted the following roles:

- `User Import` - Allows the utility to create and update users, and associated records, in the Hornbill Platform.
- `Basic User Role` - Allows the utility to Core basic user roles.
- `Self Service User` - Allows the utility to assign Service Manager roles.
- `Board BPM Access` - Allows the utility to assign Board Manager roles.
- `Docmanager Portal` - Allows the utility to assign Document Manager roles.

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

For the import utility to access LDAP data, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

### Key Creation

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`;
* Click `+ Create New Key`
* Choose a key type of `LDAP Authentication`
* Give the KeySafe key a Title
* Optionally add a Description
* Populate the following fields on the form:
  * `Host` - The IP address or hostname of your LDAP host.
  * `Port` - The port to access your LDAP through. 389 (LDAP) and 636 (LDAPS) are commonly used values.
  * `Username` - The username of the account that should be used to authenticate the connection to your LDAP.
  * `Password` - The password for the above account. 
* Click `Create Key`

Once the key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

### Key Example

<img src="/_books/data-imports-guide/users/ldap/images/ldap_user_import_keysafe.png" width="650px" alt="KeySafe LDAP Example"/>
