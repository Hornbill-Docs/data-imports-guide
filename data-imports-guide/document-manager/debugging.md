# Troubleshooting Document Imports

## Logs
All logging output is saved in the `log` folder, in the same location as the executable. The file name contains the date and time the import was run for example
`docimport_201511061426130000.log`

## Common Errors

Below are some common errors that you may encounter in the user imports log file and what they mean:

- `Get https://api.github.com/repos/hornbill/document/tags: dial tcp xx.xx.xx.xx:xxx: ...`
This most likely indicates that you have an HTTP proxy server on your network between the host running the executable and your Hornbill API endpoint. Ensure the HTTPS_PROXY environment variable is set, and that the proxy is configured to allow this communication. See the HTTP Proxies content from the Network section of the Overview article for more information.
- `Database Query Error: driver: bad connection.`
    - This can be associated with an incorrect driver in the configuration file, or an username, and/or password in the KeySafe key used for authenticating requests into your data source.
- `Unable to write to log Post /system/?method=logMessage: unsupported protocol scheme.`
    - This suggests that the import was unable to access the CSV file via ODBC. There could be two reasons for this:
        - Someone had moved the CSV from its default location
        - The CSV file was open at the time and this locked the import