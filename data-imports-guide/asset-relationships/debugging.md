# Troubleshooting Asset Imports

## Logging

Logging output is saved in the log directory in the same directory as the executable the file name contains the date and time the import was run `asset_rel_log_20190925140000.log`

## Common Errors

Below are some common errors that you may encounter in the user imports log file and what they mean:

* `Error processing authentication details: Decryption of authentication data failed: Key not valid for use in specified state.`
  * This error can occur for either of the below states and can be resolved by either running the import as the same session user and on the same computer that the original import authentication took place, or by [resetting the encrypted credentials](/data-imports-guide/asset-relationships/command#resetting-encrypted-credentials):
    * When the user who runs the import is not the same user who first ran the import;
    * When the import is run on a different computer from the one that originally performed the authentication details encryption. 
* `Error Decoding Configuration File:...`
    * This will typically be due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
* `API Call failed when retrieving record:Post https://mdh-p01-api.hornbill.com/[instanceName]/xmlmc/data/?method=entityBrowseRecords: dial tcp 78.129.xxx.xxx:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond`
  * This is typically encountered if you use a proxy for all of your internet traffic and you haven't set the `HTTP_PROXY` environment variable as described in the [Network](/data-imports-guide/asset-relationships/overview#network) section of the Overview article.
* `Database Query Error: driver: bad connection.`
    * This can be associated with an incorrect driver in the configuration file, or an username, and/or password in the KeySafe key used for authenticating requests into your data source.
* `Unable to write to log Post /system/?method=logMessage: unsupported protocol scheme.`
    * This suggests that the import was unable to access the CSV file via ODBC. There could be two reasons for this:
        * Someone had moved the CSV from its default location
        * The CSV file was open at the time and this locked the import