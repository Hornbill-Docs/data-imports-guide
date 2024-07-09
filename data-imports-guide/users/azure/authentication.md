# Authentication

The User Import - Azure utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store Entra ID credentials.

## API Keys

[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/users-apikeys.md]]

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

For the import utility to access Entra ID data, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

### Entra ID App Registration

Before creating a KeySafe Key, you will first need details of an appropriate App Registration from your Entra ID tenant. Your Entra ID administrator will be able to help you with this, but the basic steps to create this are as follows:

* Log into your Azure portal
* Navigate to `Azure Active Directory`
* In the `App registrations` menu item, click the `New Registration` button
* Provide a name for the application, select the appropriate account type, and click the `Register` button
* Once created, several API permissions need to be applied to the app. We have found that the following permissions need to be granted within Azure, though these could differ for your tenant so please rely on your internal Azure expertise:
  * Application Permissions:
    * `Group.Read.All`
    * `GroupMember.Read.All`
    * `Team.ReadBasic.All`
    * `TeamMember.Read.All`
    * `User.Read.All`
  * Delegated Permission:
    * `User.Read`
* The permission settings need confirming by clicking `Grant admin consent`
* The `Client ID` and `Tenant ID` can be found and copied from within the Overview section (as `Application (client) ID` and `Directory (tenant) ID` respectively)
* Next, you will need to generate a `Client Secret`, by navigating to `Certificates & Secrets` in the menu, clicking `New Client Secret`, and providing a description and expiry date, before clicking the `Add` button
* Copy the `Value` of the Client Secret before continuing 

### Key Creation

Now you have a Client ID, Tenant ID and Client Secret for your Entra ID app registration, you can create a KeySafe key as so:

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`;
* Click `+ Create New Key`
* Choose a key type of `Azure Imports`
* Give the KeySafe key a Title
* Optionally add a Description
* Populate the `Tenant ID`, `Client ID` and `Client Secret` inputs with the values copied above
* Click `Create Key`

Once the key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.