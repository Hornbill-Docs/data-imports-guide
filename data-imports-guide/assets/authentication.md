# Authentication

The Asset Import utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store credentials for the asset data source.

## API Keys

For the utility to read, create and update records via the Hornbill API, it requires an API Key to be securely stored alongside the client.

### User

Every action within Hornbill must be performed in the context of a user account. The user account must possess roles for the platform and applications that you are granting access to via the import utility. The above comment about roles refers to the Hornbill Security Model when associating roles with user accounts. This security measure prevents you from inflating your session rights or granting a user more rights than you have yourself.

:::important
We strongly recommend that you create a Service Account in your Hornbill instance, and API Keys against that account which can then be used to perform the required API calls back into Hornbill. 

Please read the API Key documentation and best practice guide before creating API keys against your user records.
:::

The service account that you create must be of type `User` (not `Basic`), and be granted the following roles:

- **User Role** - Allows the utility to perform entity actions in the Hornbill platform.
- **Asset Management User** - Allows the utility to create and update Asset Management records in Service Manager.
- **Hornbill Service Manager Integrations** - Grants several privileges for access to entities and the execution of stored queries. ***NOTE*** - This role is only intended for accounts that are used for integrations or to perform data imports, and should not be applied to interactive user accounts. 

### API Key Rules

The Asset Imports require access to the following Hornbill Platform and application APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

```cmd
admin:getApplicationList
admin:groupGetList2
admin:keysafeGetKey
bpm::iBridgeInvoke
data:entityAddRecord
data:entityAddRecords
data:entityBrowseRecords2
data:entityDeleteRecord
data:entityUpdateRecord
data:queryExec
session:getApplicationList
system:logMessage
apps/com.hornbill.core:getSitesList
apps/com.hornbill.servicemanager/AssetUsers:create
apps/com.hornbill.servicemanager/AssetUsers:delete
apps/com.hornbill.servicemanager/AssetUsers:read
apps/com.hornbill.suppliermanager/SupplierAssets:addSupplierAsset
apps/com.hornbill.suppliermanager/SupplierContractAssets:addSupplierContractAsset
```

## KeySafe

For the import utility to access data from your source database, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

Once the relevant key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

:::important
When you have created your KeySafe Key, note down the KeySafe Key ID which can be found in the URL when you are on the key details form in your browser, as this will be needed when configuring your imports. In the example below, `4` is the KeySafe Key ID:

`https://live.hornbill.com/yourinstanceid/admin/platform/security/keysafe/4/`
:::

### Key Types

As the Asset Import utility supports the import of asset data from many different data sources, it must therefore also support many different KeySafe Key types:

* [Azure Resource Query](/data-imports-guide/assets/authentication#key-type-azure-resource-query) - and optionally (for importing Software Inventory records against assets) [Azure Log Analytics](/data-imports-guide/assets/authentication#key-type-azure-resource-query). Used for the following data sources:
  * `azureresourcequery` - Azure Resource Query, including data from Azure Arc.
* [Certero](/data-imports-guide/assets/authentication#key-type-certero) - Used for the following data sources:
  * `certero` - Certero IT Asset Management.
* [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication) - Used for the following data sources:
  * `mssql` - Microsoft SQL Server (2005 or above).
  * `mysql` - MySQL 4.1 or above, or any version of MariaDB.
  * `mysql320` - MySQL Server v3.2.0 to v4.0.
  * `odbc` - ODBC driver.
  * `swsql` - Supportworks SQL (Core Services v3.x).
* [Google Workspace](/data-imports-guide/assets/authentication#key-type-google-workspace) - Used for the following data sources:
  * `google` - Google Workspace Enterprise Chrome OS.
* [Jamf](/data-imports-guide/assets/authentication#key-type-jamf) - Used for the following data sources:
  * `jamf` - Jamf 
* [LDAP Authentication](/data-imports-guide/assets/authentication#key-type-ldap) - Used for the following data sources:
  * `ldap` - LDAP data sources (including Active Directory).
* [oAuth 2.0](/data-imports-guide/assets/authentication#key-type-oauth-20) - Used for the following datasources:
  * `manage engine` - Manage Engine
* [Username + Password](/data-imports-guide/assets/authentication#key-type-username-password) - Used for the following data sources:
  * `nexthink` - Nexthink.
* [Virima](/data-imports-guide/assets/authentication#key-type-virima) - Used for the following data sources:
  * `virima` - Virima.
* [VMWare Workspace One UEM](/data-imports-guide/assets/authentication#key-type-vmware-workspace-one-uem) - Used for the following data sources:
  * `workspaceone` - VMware Workspace One UEM.

:::tip
The `csv - CSV / Text file(s)` data source reads its data from the file system (local or network), and therefore does not require Keysafe keys.
:::

### Key Type - Azure Log Analytics

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Azure Log Analytics`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Click `Create Key`.
* Once the Key is created, you will need to connect to Azure Log Analytics and your account, in order to authorize the Hornbill App to perform the listed Azure Log Analytics requests. Click `Connect` and you will be redirected to the Azure authentication page in a popup window.
* Log in to your Azure Log Analytics account, and then you will be prompted to review the operations you are authorizing the Hornbill App to be allowed to perform with the chosen Azure Log Analytics account.
* Review the scopes/permissions, and click `Continue`. You will then be returned to your KeySafe key.
* 
### Key Type - Azure Resource Query

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Azure Resource Query`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Click `Create Key`.
* Once the Key is created, you will need to connect to Azure Resource Query and your account, in order to authorize the Hornbill App to perform the listed Azure Resource Query options. Click `Connect` and you will be redirected to the Azure authentication page in a popup window.
* Log in to your Azure Resource Query account, and then you will be prompted to review the operations you are authorizing the Hornbill App to be allowed to perform with the chosen Azure Resource Query account.
* Review the scopes/permissions, and click `Continue`. You will then be returned to your KeySafe key.

### Key Type - Certero

Keys of this type require a Certero API Key to be created against a user account that has permission to fetch assets before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Certero`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `API Key Name` - The name of the API Key associated with a user account that has permission to fetch assets.
  * `API Key` -  An API Key associated with a user account that has permission to fetch assets.
  * `API Endpoint` - The API Endpoint used to access the assets records contained in your Cerero instance, for example: `https://yourinstance.cloud.certero.com/api/odata/ComputerSystem`
* Click `Create Key`.

### Key Type - Cynerio

Keys of this type require a Cynerio API Key to be created against a user account that has permission to fetch assets before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Cynerio`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Access Token URL` - Your Cynerio account-specific authentication URL, in the format: `https://your-portal-login.cynerio.com`
  * `API URL` - Your Cynerio account-specific API endpoint URL, in the format: `https://your-cynerio-account.cyner.io`
  * `Client ID` -  The Client ID for your Cynerio integration application
  * `Client Secret` -  The Client Secret for your Cynerio integration application
* Click `Create Key`.

### Key Type - Database Authentication

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Database Authentication`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Server` - The IP address or hostname of your database host.
  * `Port` - The port used to connect to your database.
  * `Database` - The database name/ID.
  * `Username` - The username of the account that should be used to authenticate the connection to your database.
  * `Password` - The password for the above account. 
* Click `Create Key`.

### Key Type - Google Workspace

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Google Workspace`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Click `Create Key`.
* Once the Key is created, you will need to connect to Google Workspace and your account, in order to authorize the Hornbill App to perform the listed Google Workspace options. Click `Connect` and you will be redirected to Google Workspace in a popup window.
* Log in to your Google Workspace account, and then you will be prompted to review the operations you are authorizing the Hornbill App to be allowed to perform with the chosen Google Workspace account.
* Select the scopes/permissions relevant to the import, and click `Continue`. You will then be returned to your KeySafe key.

### Key Type - Jamf

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Jamf`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* `Username` - The username of your account should be used to authenticate the connection.
* `Jamf Server` - The name that appears in the url when you login to Jamf, for example: "https://'JAMF SERVER NAME'.jamfcloud.com/". Please only include the name as it appears in the URL.
* `Password` - The password associated with the above username.
* Click `Create Key`.

### Key Type - Microsoft Intune

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Microsoft Intune`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Click `Create Key`.
* Once the Key is created, you will need to connect to Microsoft and your account, in order to authorize the Hornbill App to perform the listed Intune options. Click `Connect` and you will be redirected to Microsoft in a popup window.
* Log in to your Microsoft Intune account, and then you will be prompted to review the operations you are authorizing the Hornbill App to be allowed to perform with the chosen Intune account.
* Select the scopes/permissions relevant to the import, and click `Continue`. You will then be returned to your KeySafe key.

### Key Type - LDAP

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `LDAP Authentication`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Host` - The IP address or hostname of your LDAP host.
  * `Port` - The port to access your LDAP through. 389 (LDAP) and 636 (LDAPS) are commonly used values.
  * `Username` - The username of the account that should be used to authenticate the connection to your LDAP.
  * `Password` - The password for the above account. 
* Click `Create Key`.

### Key Type - oAuth 2.0
* In Honbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `oAuth 2.0`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Client Id` - The client id associated with the Zoho Server Based application that will be used to authenticate the connection.
  * `Client Secret` - The client secret associated with the Zoho Server Based application that will be used to authenticate the connection.
  * `oAuth Scope` - This field needs to be set to 'DesktopCentralCloud.Inventory.READ'.
  * `Authorization URL` - This fields needs to be set to `https://accounts.zoho.{YOUR-DOMAIN}/oauth/v2/auth`.
  * `Access URL` - This field needs to be set to `https://accounts.zoho.{YOUR-DOMAIN}/oauth/v2/token`.
  * `Resposne Type` - This field needs to be set to 'code'.
  * `Additional Params` - This field needs to be set to 'access_type=offline'.
  * `Grant Type` - This field needs to be set to 'authorization_code'.
  * `Refresh URL` - This field needs to be set to `https://accounts.zoho.{YOUR-DOMAIN}/oauth/v2/token`.
  * `Refresh Grant Type` - This fields needs to be set to 'refresh_token'.
  * `API Endpoint` - The API endpoint your instance points to when calling functions this would include the domain specific to your location. More information about Manage Engine domains can be found [here](https://www.manageengine.com/products/desktop-central/api/cloud_index.html). An example of a UK based domain API endpoint would be `https://endpointcentral.manageengine.uk`.
* Click `Create Key`.

:::note
For Manage Engine integrations the 'client id' and 'client secret' will need to be set up [here](https://api-console.zoho.com). Please select a server based application when setting up and if the Key Safe Key is revoked a new sever based applicaiton will need to be created and the 'client id' and 'client secret' will need to be updated as the refresh token is only generated once. 
:::
### Key Type - Username + Password

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Username + Password`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Username` - The username of the account that should be used to authenticate the connection to your account.
  * `Password` - The password for the above account. 
  * `API Endpoint` - The service API endpoint.
* Click `Create Key`.

### Key Type - Virima

Keys of this type require an API Key and Tenant ID to be created and provided to you by Virima. This API key needs permission to fetch CIs and software inventory records, before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Virima`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `API Key Name` - The name of the API Key associated with a user account that has permission to fetch assets.
  * `Tenant ID` - The Tenant ID as provided by Virima.
  * `API Key` -  The API Key as provided by Virima.
  * `API Endpoint` - The API Endpoint for your Virima tenant, for example: `https://login-euc1.virima.com/www_em/rest`
* Click `Create Key`.

### Key Type - VMWare Workspace One UEM

VMware Workspace One UEM requires an oAuth Client to be created before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility. See the [VMware client creation documentation](https://docs.vmware.com/en/VMware-Workspace-ONE-UEM/services/UEM_ConsoleBasics/GUID-UsingUEMFunctionalityWithRESTAPI.html) for more information.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `VMWare Workspace One UEM`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Domain` - The Domain with your Workspace One UEM domain, for example, `https://cn1498.awmdm.com`.
  * `Region` - The region ID where your VMware Workspace One UEM account is hosted. This can be found in the VMware Workspace One UEM URL, for example, `emea`.
  * `Client ID` - The Client ID of your VMware oAuth Client.
  * `Client Secret` - The Client Secret of your VMware oAuth Client.
* Click `Create Key`.
