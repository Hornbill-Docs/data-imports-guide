# Debugging

## Testing Overview
If you run the application with the argument -dryrun=true then no requests will be raised within Service Manager - the XML used to raise requests will be saved in the log file so you can ensure the database mappings are correct before running the import.

``goHornbillRequestImport.exe -dryrun=true -file=conf.json``

## Trouble Shooting
### Logging Overview
All logging output is saved in the log directory, in the same directory as the executable. The file name contains the date and time the import was run **request_import_2015-11-06T14-26-13Z.log**

#### **Common Error Messages**
Below are some common errors that you may encounter in the log file and what they mean:

- **[ERROR] Error Decoding Configuration File:.....** - this will be typically due to a missing quote (") or comma (,) somewhere in the configuration file. This is where an online JSON viewer/validator can come in handy rather than trawling the conf file looking for that proverbial needle in a haystack.
- **[ERROR] Database Query Error: read tcp 127.0.0.1:xxxx->127.0.0.1:xxxx: wsarecv: An established connection was aborted by the software in your host machine.** - This is most likely due to an incorrect Username and/or password specified in the AppDBConf section of the conf file. Check and confirm the username and password used to access your database.
- **[ERROR] Database Query Error: driver: bad connection.** - Like the error above, this can be associated with an incorrect Username and/or password specified in the AppDBConf section of the conf file. Check and confirm the username and password used to access your database.

#### **Error Codes**
- 100 - Unable to create log File
- 101 - Unable to create log folder
- 102 - Unable to Load Configuration File