# Troubleshooting LDAP User Imports

## Logs

All logs are saved in the `log` folder, which can be found in the same location as the executable. The log file names contain the date and time the import was run, for example: `LDAP_User_Import_2023-08-30T16-19-51+01-00.log`.

## Common Errors

Below are some common errors that you may encounter in the user imports log file and what they mean:

* `Error processing authentication details: Decryption of authentication data failed: Key not valid for use in specified state.`
  * This error can occur for either of the below states and can be resolved by either running the import as the same session user and on the same computer that the original import authentication took place, or by [resetting the encrypted credentials](/data-imports-guide/users/ldap/command#resetting-encrypted-credentials):
    * When the user who runs the import is not the same user who first ran the import;
    * When the import is run on a different computer from the one that originally performed the authentication details encryption. 
* `Get https://api.github.com/repos/hornbill/user-import-ldap/tags: dial tcp xx.xx.xx.xx:xxx: ...`
  * This most likely indicates that you have an HTTP proxy server on your network between the host running the executable and your Hornbill API endpoint. Ensure the HTTPS_PROXY environment variable is set, and that the proxy is configured to allow this communication. See the HTTP Proxies content from the [Network](/data-imports-guide/users/ldap/overview#network) section of the Overview article for more information.
[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/users_troubleshooting.md]]
