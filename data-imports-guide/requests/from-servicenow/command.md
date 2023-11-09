# Execute

## Command Line Parameters
- file - Defaults to conf.json - Name of the Configuration file to load
- dryrun - Defaults to false - Set to true, and the XMLMC for new request creation will not be called and instead the XML will be dumped to the log file, this is to aid in debugging the initial connection information.
- debug - Defaults to false - Set this to true, and the log file will include additional debugging information. NOTE! The log file can increase in size dramatically with this flag set to true!
- zone - Defaults to eur - Allows you to change the ZONE used for creating the XMLMC EndPoint URL https://{ZONE}api.hornbill.com/{INSTANCE}/
- concurrent - defaults to 1. This is to specify the number of requests that should be imported concurrently, and can be an integer between 1 and 10 (inclusive). 1 is the slowest level of import, but does not affect performance of your Hornbill instance, and 10 will process the import more quickly but may affect performance of your instance.
- attachments - defaults to true. By default, all attachments associated with the tasks that you import will be imported in to Service Manager and associated with the relevant requests. Set this to false to prevent any file attachments being imported.

## Testing Overview
If you run the application with the argument dryrun=true then no requests will be logged - the XML used to raise requests will instead be saved in to the log file so you can ensure the data mappings are correct before running the import.

``goServiceNowRequestImport.exe -dryrun=true``