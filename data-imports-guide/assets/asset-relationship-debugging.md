# Troubleshooting Asset Imports

## Logging

Logging output is saved in the log directory in the same directory as the executable the file name contains the date and time the import was run ``asset_rel_log_20190925140000.log``

## Common Errors

Below are some common errors that you may encounter in the user imports log file and what they mean:

- ``Error Decoding Configuration File:...``
    - This will typically be due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
- ``https:// ........invalid request :path "/xmlmc/apps/com.hornbill.servicemanager/?method=[methodName]"``
    - If you identify errors stating an “invalid request path” for one or more API calls, this is typically due to a missing or incorrect instance name specified in the conf.json file. Check the instance id is correct. It also may be prudent to check you have added a valid API key too.
- ``Database Query Error: driver: bad connection.``
    - This can be associated with an incorrect driver in the configuration file, or an username, and/or password in the KeySafe key used for authenticating requests into your data source.
- ``Unable to write to log Post /system/?method=logMessage: unsupported protocol scheme.``
    - This suggests that the import was unable to access the CSV file via ODBC. There could be two reasons for this:
        - Someone had moved the CSV from its default location
        - The CSV file was open at the time and this locked the import