# Execute

## Command line Parameters

- file - Defaults to `conf.json` - Name of the Configuration file to load
- dryrun - Defaults to `false` - Set to True and the XMLMC for the raising of new requests will not be called, and instead the generated XML for each request will be dumped to the log file. This is to aid in debugging the initial connection information.
- debug - Defaults to `false` - set to true to output additional debug logging
- concurrent - defaults to `1`. This is to specify the number of requests that should be imported concurrently, and can be an integer between 1 and 10 (inclusive). 1 is the slowest level of import, but does not affect performance of your Hornbill instance, and 10 will process the import much more quickly but could affect performance.
- custorg - defaults to `false` - When set to `true`, the company and organisation mappings will be ignored, and the tool will use the Contacts Organisation (if the customer is of type Contact (1)), or the Users Home Organisation (if the customer is of type User (0)), when logging the requests