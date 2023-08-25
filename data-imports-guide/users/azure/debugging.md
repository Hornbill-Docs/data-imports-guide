# Troubleshooting

## Common Errors

Below are some common errors that you may encounter in the log file and what they mean:
* `[ERROR] Error Decoding Configuration File:...`
  * This will typically be due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
* `[ERROR] Get https://api.github.com/repos/hornbill/user-import-azure/tags: dial tcp xx.xx.xx.xx:xxx: ...`
  * This most likely indicates that you have an HTTP proxy server on your network between the host running the executable and your Hornbill API endpoint. Ensure the HTTPS_PROXY environment variable is set, and that the proxy is configured to allow this communication. See the HTTP Proxies content from the [Network](/data-imports-guide/users/azure/overview#network) section of the Overview article for more information.
* `panic: runtime error: invalid memory address or nil pointer deference [recovered]...`
  * This error suggests an incorrectly specified attribute in the conf file. Where information is being obtained from a directory attribute, the attribute must be in the following format: `{{.directoryAttributeName}}`
* `[ERROR] Unable to Create User: Invalid value for parameter '[parameter name]': The text size provided (31 characters) is greater than the maximum allowable size of 20 characters for column [column name]`
  * The contents of your directory attribute exceed the maximum number of characters that can be placed in the Hornbill database column.
* `[ERROR] Unable to Create User: The value in element <userId> did not meet the required input pattern constraints. at location '/methodCall/params/userId' `
  * The User ID contains characters that are not allowed. Please see the [accountIdType documentation](/esp-api/types/simple/accountIdType) for the User ID requirements. 
* `[ERROR] Unable to Create User: [usedID] Error: The specified handle [Display Name] is already in use`
  * By default, the "Handle" (Hornbill Display Name) must be unique. This error suggests a user account already exists in Hornbill which is using this handle. The duplicate-handle validation can be disabled via a setting found in Hornbill Configuration, under `Home > System > Advanced Settings` and filtering for `api.xmlmc.uniqueUserHandle.enable`
* `[ERROR] Unable to Update User: Invalid value for parameter '[parameter name]': Error setting value for column '[column name]'. bad lexical cast: source type value could not be interpreted as target`
  * This error indicates that the contents of your directory attribute are in a format that is not compatible with the type of the Hornbill database column. For example, you will get this when trying to place text into a database field that is of type "INT" (accepts integer values only).
* `[ERROR] Unable to Load LDAP Attribute: '[LDAP attribute name]' For Input Param: '[Hornbill Parameter name]' `
  * When the import utility is unable to load a particular LDAP attribute, this means that the attribute field in your directory does not contain a value. This error will not prevent the user account from being created or updated in Hornbill and can be considered more as a warning rather than an outright failure or problem.
* `[ERROR] Unable to Set User Status [status name]: You have reached your user subscription limit of [xx], you will need to expand your subscription level if you wish to add more users`
  * The utility is trying to update the user status of an existing user account from an inactive status (i.e. "archived" or "suspended") to "active". However, for this to be successful you must have some subscriptions available.
* `[ERROR] Unable to run import, a previous import is still running`
  * This can occur if the previous import has failed to complete. Perform a manual (non-scheduled) run of the import from the command line including the argument `-forcerun=true`. Future imports should now run without issue.