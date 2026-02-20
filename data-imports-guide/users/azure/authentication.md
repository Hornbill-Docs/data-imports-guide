# Authentication

The User Import - Azure utility uses API keys to authenticate all API calls into Hornbill instances. It uses KeySafe to store Microsoft Entra ID credentials securely.

## API keys

The User Import - Azure utility requires specific permissions to interact with your Hornbill instance. You must configure your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) to include the following Hornbill Platform APIs. You can also add IP rules to increase security.

### Required API key rules

* `activity:profileImageSet`
* `admin:keysafeGetKey`
* `admin:sysOptionGet`
* `admin:userAddGroup`
* `admin:userAddRole`
* `admin:userCreate`
* `admin:userDeleteGroup`
* `admin:userProfileSet`
* `admin:userSetAccountStatus`
* `admin:userUpdate`
* `data:entityAddRecord`
* `data:entityGetRecord`
* `data:entityUpdateRecord`
* `data:queryExec`
* `session:getSystemLicenseInfo`

## KeySafe

The import utility requires authentication credentials stored in KeySafe to access Microsoft Entra ID data.

Review the [KeySafe documentation](/esp-fundamentals/security/keysafe) before you store credentials.

### Register an Entra ID application

Before you create a KeySafe key, you must obtain details from an App Registration in your Microsoft Entra ID tenant. If you do not have administrative access to Azure, contact your Microsoft Entra ID administrator for assistance.

#### Registration steps

1. Sign in to the **Azure portal**.
2. Go to **Microsoft Entra ID**.
3. Select **App registrations** from the side menu.
4. Select **New Registration**.
5. Enter a name for the application.
6. Select the appropriate account type.
7. Select **Register**.
8. Select **API permissions** from the menu to apply the required permissions.
9. Grant the following **Application Permissions**:
    * `Group.Read.All`
    * `GroupMember.Read.All`
    * `Team.ReadBasic.All`
    * `TeamMember.Read.All`
    * `User.Read.All`
10. Grant the following **Delegated Permission**:
    * `User.Read`
11. Select **Grant admin consent** to confirm the permission settings.
12. Go to the **Overview** section.
13. Copy the **Application (client) ID** and the **Directory (tenant) ID**.
14. Select **Certificates & secrets** from the menu.
15. Select **New client secret**.
16. Enter a description, select an expiry date, and select **Add**.
17. Copy the **Value** of the client secret.

### Create a KeySafe key

Use the Client ID, Tenant ID, and Client Secret from your Microsoft Entra ID app registration to create the KeySafe key in Hornbill.

#### KeySafe creation steps

1. In Hornbill, go to **Configuration** > **Platform Configuration** > **KeySafe**.
2. Select **+ Create New Key**.
3. Select **Azure Imports** as the key type.
4. Enter a **Title** for the KeySafe key.
5. Optional: Enter a **Description**.
6. Enter the **Tenant ID**, **Client ID**, and **Client Secret** values you copied from the Azure portal.
7. Select **Create Key**.

#### Expected Result

The key appears in your KeySafe list. You can now restrict access to this key so only the API key created for your service account can use it. For more information, see the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) regarding access control.
