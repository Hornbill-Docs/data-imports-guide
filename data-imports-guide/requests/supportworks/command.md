# Execute

## Command line Parameters

- file - Defaults to `conf.json` - Name of the Configuration file to load
- dryrun - Defaults to `false` - Set to True and the XMLMC for the raising of new requests will not be called, and instead the generated XML for each request will be dumped to the log file. This is to aid in debugging the initial connection information.
- debug - Defaults to `false` - set to true to output additional debug logging
- concurrent - defaults to `1`. This is to specify the number of requests that should be imported concurrently, and can be an integer between 1 and 10 (inclusive). 1 is the slowest level of import, but does not affect performance of your Hornbill instance, and 10 will process the import much more quickly but could affect performance.
- custorg - defaults to `false` - When set to `true`, the company and organization mappings will be ignored, and the tool will use the Contacts Organization (if the customer is of type Contact (1)), or the Users Home Organization (if the customer is of type User (0)), when logging the requests

## Testing Overview

There is no substitute for hands-on experience when becoming familiar with the Hornbill import utilities.
The Supportworks Request import accepts and understands a number of "Command Line Parameters" that can be used when running the utility from the command line. The most important one for testing is the -dryrun=true command. When this is specified, no information will be written to Hornbill and it allows you to confirm that the configuration is correct and that a connection to your Supportworks server can be established. A dryrun still outputs a log file which provides you with an opportunity to review and understand any error messages that may occur.

**Suggested Testing Approach**
Below are some high level steps to help you build confidence in your configuration:

1. Prepare the priority, team, category, and resolution category Supportworks to Service Manager mappings at the bottom of the conf file as required.
2. Start by focusing on a single call class section e.g Incidents (ensure all other sections are set to "Import:false")
3. Open the Supportworks Query tool on your Supportworks Server and run the default SQL statement contained in the conf.json file.
4. Amend the SQL query to remove references to non-existent columns (the database schema varies between Supportworks editions) until the query runs successfully and returns some results.
5. Add a limit to the query e.g. LIMIT 30 (Its good practice to initially test on a small set of request records as this allows the dryruns to complete quicker and there is less log content to review).
6. Copy your new query back into the conf file.
7. Perform a dryrun (by executing the utility along with the -dryrun=true command line parameter).
8. Review cmd output and log file for errors
9. Check against "Common Error Messages" listed on the wiki and take action to rectify these as well as amending any of the priority, team, and category mappings where necessary.
10. Continue with dryrun tests until you are happy that all the errors are accounted for.
11. Perform a live run with this small sample of request records i.e. set -dryrun=false
12. Review an imported request record and check that all properties are as expected:
    - Team has been assigned
    - Owner is assigned
    - Status is correct
    - Customer is set
    -  Priority is set
    - Category is set
    - Call diary updates have been imported
    - Attachments have been imported and can be opened
    - Check any other data you might have decided to import.
13. Adjust conf file field mappings as necessary
14. Loop through steps 11 - 14 as many times as is necessary until you are happy with the information being transported with the imported requests.
15. Perform steps 2 to 14 for the Service Requests section and other call class sections as required.
16. Plan the live import of all the desired requests. It can take between 1 and 3 seconds ti import a request record, therefore if you have a large number (> 15,000) then it's advisable to manage the import in batches. This will involve multiple configuration files.

Example command line: ``goSWRequestImport.exe -dryrun=true -file=conf.json``