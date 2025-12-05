# Meridian Asset Tag Import
This tool allows you to update Hornbill asset records with Meridian hardware asset tag information. 

## Installation

* Download the ZIP archive containing the import executable relevant for your operating system and architecture
* Extract zip into a folder you would like the application to run from e.g. `C:\assettagimport\`
* Open `conf.json` and add in the necessary configration
* Open a Command Line/Terminal as Administrator
* Change Directory to the folder with the executable and config file `C:\assettagimport\`
* Run the command :  
   * For Windows Systems: goHornbillMeridianImport.exe  
   * For \*nix Systems: ./goHornbillMeridianImport

## Configuration

Example JSON File:

```json
{
"APIKey": "your Hornbill API Key",
   "InstanceID": "Your Hornbill Instance ID",
   "MeridianToken": "A Meridian Token",
   "LocationID": "The ID of your Meridian Location",
   "AssetMatchColumn": {
       "Meridian":"ExternalID",
       "Hornbill": "Name"
   },
   "ImportMapping": {
       "h_location":"Location",
       "h_external_id":"ID",
       "h_external_source":"Source"
   }
}
```

* `APIKey` \- a Hornbill API key for a user account with the correct permissions to carry out all of the required API calls
* `InstanceId` \- the Hornbill Instance ID (case sensitive)
* `MeridianToken` \- an application token generated within your Meridian account, that has read-only access to your Asset Tags. See the \[Meridian documentation\] for more information
* `LocationID` \- the ID of the location to retrieve asset tag information from
* `AssetMatchColumn` \- The Meridian and Hornbill asset columns to match records agaainst  
   * `Meridian` \- the Meridian column that holds a value to match what is stored in the Hornbill column, below. Supported values:  
         * `ExternalID` \- The External ID from the Meridian asset tag record  
         * `ID` \- The ID from the Meridian asset tag record  
         * `Name` \- The Name from the Meridian asset tag record  
   * `Hornbill` \- the Hornbill column that holds a value to match what is stored in the Meridian column, above. Supported values:  
         * `PrimaryKey` \- The Primary Key from the Asset record  
         * `Description` \- The Description field from the Asset record  
         * `Name` \- The Name field from the Asset record  
         * `Tag` \- The Asset Tag field from the Asset record
* `ImportMapping` \- The mapping information to determine which Meridian column values to populate the Hornbill asset record columns with. The parameter name is the Hornbill asset column, and the value is the Meridian asset tag column. Meridian supported columns:  
   * * `ExternalID` \- The External ID of the Meridian asset tag record  
         * `ID` \- The ID of the Meridian asset tag record  
         * `Location` \- The Location ID of the Meridian asset tag record  
         * `Name` \- The Name of the Meridian asset tag record  
         * `Mac` \- The MAC Address of the Meridian asset tag record  
         * `Source` \- The string **Meridian**

## API Key Rules

This utility uses (API keys):

* data:entityUpdateRecord
* data:getRecordCount
* data:queryExec

### Execute

## Command Line Parameters

* `file` \- Defaults to `conf.json` \- Name of the Configuration file to load
* `dryrun` \- Defaults to `false` \- Set to `true` and the XML for all XMLMC operations will be dumped to the log file, and any CREATE or UPDATE operations will be skipped. This is to aid in debugging the initial connection information.
* `version` \- Defaults to `false` \- when set to `true`, the tool will output its version number before exiting

## Testing

If you run the application with the argument dryrun=true then no asset updates will occur the XML used to create or update will be saved in the log file so you can ensure the data mappings are correct before running the import.

`goHornbillMeridianImport.exe -dryrun=true` 

## Scheduling

### Windows

You can schedule goHornbillMeridianImport.exe to run with any optional command-line argument from Windows Task Scheduler:

* Ensure the user account running the task has rights to goHornbillMeridianImport.exe and the containing folder.
* Make sure the Start In parameter contains the folder where goHornbillMeridianImport.exe resides in otherwise it will not be able to pick up the correct path.

## Logging

All Logging output is saved in the log directory in the same directory as the executable the file name contains the date and time the import was run `meridianAssetImport20190925140000.log` 



