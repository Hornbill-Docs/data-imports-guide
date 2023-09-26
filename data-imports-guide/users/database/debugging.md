# Troubleshooting Database User Imports

## Logs

All logs are saved in the `log` folder, which can be found in the same location as the executable. The log file names contain the date and time the import was run, for example: `DB_User_Import_2023-08-30T16-19-51+01-00.log`.

## Common Errors

Below are some common errors that you may encounter in the user imports log file and what they mean:

* `Error Decoding Configuration File:...`
  * This will typically be due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
* `Get https://api.github.com/repos/hornbill/user-import-database/tags: dial tcp xx.xx.xx.xx:xxx: ...`
  * This most likely indicates that you have an HTTP proxy server on your network between the host running the executable and your Hornbill API endpoint. Ensure the HTTPS_PROXY environment variable is set, and that the proxy is configured to allow this communication. See the HTTP Proxies content from the [Network](/data-imports-guide/users/database/overview#network) section of the Overview article for more information.
[[INCLUDE https://raw.githubusercontent.com/Hornbill-Docs/hdoc-library/main/hdoc-library/dataimports/users_troubleshooting.md]]