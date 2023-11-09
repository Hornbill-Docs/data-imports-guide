# Configuration

## Overview

This tool uses one or more CSV files to import documents into Document Manager. These CSV files contain metadata required to perform the imports. Demonstration CSV files are provided within the package.

### csvd
The CSV file name provided for this command line argument (-csvd) is the main template and is used to create the documents in Hornbill. It contains the main details of the files being imported. An example csv template is provided in the download package and must always contain the 6 columns listed below. If you don't wish to use the columns, just leave the rows blank but make sure the column name always exists in the csv file, in the order listed:

- `Filepath`: MANDATORY - The full filepath of the file being imported into a document
- `Title`: Optional - The title of the new document. If an empty string is provided, then the name of the source file (minus the extension) will be used as the document title
- `Status`: MANDATORY - The status of the new document. Can be active, draft or retired
- `Description`: Optional - The description of the new document
- `ReviewDate`: Optional - The next review date for the new document (must be in the format "**YYYY-mm-dd HH:mm:ss**")
- `VersioningEnabled`: MANDATORY - true/false, should versioning be enabled for the new document
- `Owner`: Optional - The User ID of the document owner. Leave blank if the owner should remain as the user whose API key is being used to run the utility. NOTE: Basic Hornbill user account cannot be document owners, only Full user accounts can.

### csvc
The CSV file name provided by the csvc argument contains the Collections that the files being imported should be added to. The CSV should contain 2 columns, and a header row. The columns should be, and in this order:

- `Filepath`: MANDATORY - The full filepath of the file being imported into a document - this should match Filepath values from the csvd file
- `Collection`: MANDATORY - The Primary Key ID of the Collection that the new document should be added to

### csvs
The CSV file name provided by the csvs argument contains the share data that the files being imported should create. The CSV should contain 5 columns, and a header row. The columns should be, and in this order:

- `Filepath`: MANDATORY - The full filepath of the file being imported into a document - this should match Filepath values from the csvd file
- `URN`: MANDATORY - The URN of the Library or User to create the share against
- `Read`: MANDATORY - true/false, should the share allow Read permissions
- `ModifyContent`: MANDATORY - true/false, should the share allow Modify Content permissions
- `ModifyMetaData`: MANDATORY - true/false, should the share allow Modify Meta Data permissions

### csvt
The CSV file name provided by the csvt argument contains the Tags that should be associated to the files being imported. The CSV should contain 2 columns, and a header row. The columns should be, and in this order:

- `Filepath`: MANDATORY - The full filepath of the file being imported into a document - this should match Filepath values from the csvd file
- `Tag`: MANDATORY - The Tag that the new document should be added to