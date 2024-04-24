# Troubleshooting Asset Imports

## Logs

All logs are saved in the `log` folder, which can be found in the same location as the executable. The log file names contain the date and time the import was run, for example `Asset_Import_2023-08-30T16-19-51+01-00.log`.

## Common Problems

### Errors
Below are some common errors that you may encounter in the imports log file and what they mean:

* `Error processing authentication details: Decryption of authentication data failed: Key not valid for use in specified state.`
  * This error can occur for either of the below states and can be resolved by either running the import as the same session user and on the same computer that the original import authentication took place, or by [resetting the encrypted credentials](/data-imports-guide/assets/command#resetting-encrypted-credentials):
    * When the user who runs the import is not the same user who first ran the import;
    * When the import is run on a different computer from the one that originally performed the authentication details encryption. 
* `Error Decoding Configuration File:...`
  * This will typically be due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
* `Get https://api.github.com/repos/hornbill/asset/tags: dial tcp xx.xx.xx.xx:xxx: ...`
  * This most likely indicates that you have an HTTP proxy server on your network between the host running the executable and your Hornbill API endpoint. Ensure the HTTPS_PROXY environment variable is set, and that the proxy is configured to allow this communication. See the HTTP Proxies content from the [Network](/data-imports-guide/assets/overview#network) section of the Overview article for more information.
* `API Call failed when retrieving Asset Class:Post https://mdh-p01-api.hornbill.com/[instanceName]/xmlmc//data/?method=entityBrowseRecords: dial tcp 78.129.xxx.xxx:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond`
  * This is typically encountered if you use a proxy for all of your internet traffic and you haven't set the `HTTP_PROXY` environment variable as described in the [Network](/data-imports-guide/assets/overview#network) section of the Overview article.
* `Database Query Error: driver: bad connection.` 
  * This can be associated with an incorrect driver in the configuration file, or an username, and/or password in the KeySafe key used for authenticating requests into your data source. 
* `Unable to write to log Post /system/?method=logMessage: unsupported protocol scheme.`
  * This suggests that the import was unable to access the CSV file via ODBC. There could be two reasons for this:
    * Someone had moved the CSV from its default location
    * The CSV file was open at the time and this locked the import

### Other Common Questions

* ***My updates are not being performed, with the tool reporting in the log:*** `Asset match found, no details require updating`
  * The import utility will, by default, perform a comparison between new data and old data in order to decide whether or not each record requires updating. This is done to ensure that import processes are optimized so that no unnecessary API calls are made to your Hornbill instance. The decision the utility makes is, in essence, a detection of changes in the **SOURCE** data, and not any data that might have been manipulated either by using the import templates in the import utility, the asset details form in the Service Manager application itself, or by calliing the Hornbill API directly. Should you wish to override this feature, and perform updates on all detected asset matches, then please see the [`forceupdates` command line argument](/data-imports-guide/assets/command#command-line-arguments). Note, depending on your import configuration, number of source records etc, this can drastically increase the time taken to perform an import run, as an API call will be made back into your Hornbill instance to attempt an update of each asset record matched.