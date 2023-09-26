# Authentication

The User Import - Google utility uses API Keys to authenticate all API calls into Hornbill instances.

## API Keys

[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/users_apikeys.md]]

### API Key Rules

The User Imports require access to the following Hornbill Platform APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

```cmd
activity:profileImageSet
admin:sysOptionGet
admin:userAddGroup
admin:userAddRole
admin:userCreate
admin:userDeleteGroup
admin:userProfileSet
admin:userSetAccountStatus
admin:userUpdate
bpm::iBridgeInvoke
data:queryExec
```

## KeySafe

For the import utility to access data from your Google Workspace domain, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

### Key Creation

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Google Workspace`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Select Create Key to save.
* Once the Key is created, you will need to connect to Google Workspace and your account, in order to authorize the Hornbill App to perform the listed Google Workspace options. Click `Connect` and you will be redirected to Google Workspace in a popup window.
* Log in to your Google Workspace account, and then you will be prompted to review the operations you are authorising the Hornbill App to be allowed to perform with the chosen Google Workspace account.
* Select the scopes/permissions relevant to the import, and click `Continue`. You will then be returned to your KeySafe key.

Once the key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

### Key Example

**KeySafe Key**

<img src="/_books/data-imports-guide/users/google/images/google_user_import_keysafe.png" width="650px" alt="KeySafe Google Example"/>

**Google Workspace Scopes/Permissions**

<img src="/_books/data-imports-guide/users/google/images/google_user_import_permissions.png" width="650px" alt="KeySafe Google Permissions"/>