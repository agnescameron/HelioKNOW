# Re-using this tool

The iiindex is built entirely using open infrastructure (Github Actions, Google Sheets), and as such is forkable and usable by anyone with a similar project. This process of forking is something we are actively working to streamline; if you have any issues adapting this code to your use case, please let us know via Github Issues and we can help out.

## 1. Google Sheets Integration

This part is the hardest to streamline out and will likely always be necessary in some form, partly because of

### 1.1 Make a Google Sheet

The iiindex was built around (and is still predominantly managed by) a google sheet, primarily because of the ability to allow anyone to edit, and have a large number of people work collaboratively at the same time.

However -- you don't actually need public editing for this tool to work, as you've got to make a service account in any case, so if you would prefer to restrict edit access you can do so. 

### 1.3 Extending the Schema

In order to sync with a git repo, there's a few additional bits of metadata required.

Add in the fields:

- UUID -- don't put anything here yet, just leave blank. this is where the fields will get filled up. good to make this the first column
- Shortname -- here's where the names of the files corresponding to each entry will go. It's worth filling this out now with recognisable nicknames, but it is also possible to auto-generate (they just might not be very nice). make sure they're unique (next step will enforce this)! In theory the UUIDs are enough for people here, but it's nice to have recognisable names
- Timestamp -- this will record edits to the sheet, leave this blank for now

### 1.4 Data Validation (optional but recommended)

This prevents duplicate names from being added to the shortnames (if there is a copy the script will create a random name instead). Select the 'Shortnames' column, and go to Data > Data Validation. Add in the following formula under 'Custom Formula Is' (assuming the shortname column is C, if you make it a different column you just need to change the `C:C` part)

```
=COUNTIF(C:C, INDIRECT(ADDRESS(ROW(),COLUMN(),)))=1
```

### 1.5 Apps Script

In order to generate UUIDs and timestamps for the entries, a script needs to be included. 

Note: if you accidentally log in with a different Google account, it'll look like the script no longer exists -- make sure to log in with the correct account to see this. 


### 1.6 Adding a Service Account

The final step is to add an account that will allow the script to interact with the google sheet. This is necessary even if your sheet is publicly editable, because of the way google assigns permissions to API based tools.

Google have [some instructions](https://support.google.com/a/answer/7378726?hl=en) for doing this -> when you get to the APIs part, you need to enable Google Drive (rather than Google Sheets specifically), and allow both read and write permissions for this sheet. Share access to the sheet with the service account you just created.

## 2. 


## 3.