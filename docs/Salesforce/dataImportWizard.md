---
layout: page
title: Salesforce Data Import Wizard
parent: Salesforce
---


#  Using Data Import Wizard to Create or Update Preservation Objects

## Overview

Salesforce has a tool called the Data Import Wizard that allow us to make mass edits and updates to our Salesforce data. The basic idea is to export a list of records you want to update, makes the changes in a spreadsheet, then reimport the list back into Salesforce.

We almost exclusively use this tool for Preservation Object records. Do not attempt to make mass data changes to other Salesforce objects that are shared by the organization (Opportunities, Contacts, etc) without first consulting with the Tech Ops Team.

When doing this for the first time, test with ONE record first.

## Mass Updating/Data Cleanup

### Scenario: Mass Import

A collection holder mails us 4 boxes at a time, along with a spreadsheet with the tapes contained in each box. Through this method we can create a Preservation Object for each box all at once.

### Format the Data

First we need to format the data from the collection holder in the format expected by Salesforce.

_Note: Some collection holders deliver their metadata in a seperate speadsheet tab per box. For our use it’s best to move all of this information into a single tab._

* Download ![PreservationOpportunity_DataWizard_Template.csv]({{site.baseurl}}/assets/files/PreservationOpportunity_DataWizard_Template.csv), which is attached to this page and open it with OpenOffice with the following options
  - Character Set Unicode UTF-8
  - Separator Options: Uncheck all except Separated by Comma
  - Double-check that it's correct. Click OK.
* You'll need to fill in the following info for each tape
  - Opportunity
      + This is a unique text string for each opportunity. You can find the opportunity ID in the URL for the opportunity:
      + https://bavc.lightning.force.com/lightning/r/Opportunity/0062J00000tZXeEQAW/view
      + In the above sample URL for an opportunity, the ID is the bolded string of characters between Opportunity/ and /View
      + The barcode may vary slightly, but the ID will always be a string of characters that looks similar to that and is near the end of the URL
  - Box *(Required)*
      + This is a required field, even if it's just "1"
  - Barcode *(Required)*
      + When doing a mass upload make sure to set aside the barcodes that you plan to use. It's easiest to just type in the first barcode, then drag down to increment the barcode for each row
      + Only enter the barcode number, do not prepend "BAVC"
  - Format of Original
      + Make sure to use the controlled vocabulary from SalesForce
  - Client Identifier
      + For Disney, they call this Tape # (or VT Number). Transcribe this from the collection holder's spreadsheet into your CSV file
  - Label On Original *(Required)*
      + Self-explanatory. The label on the tape. This is a required field so don't leave it blank! You can put "Unlabeled" if it's left blank.
  - Embedded Metadata Fields
      + The following embedded metadata fields can have metadata from the collection holder put into them.
      + Embedded Metadata: Title
      + Embedded Metadata: Artist
      + Embedded Metadata: Date
      + Embedded Metadata: Album
      + Embedded Metadata: Comment
      + Embedded Metadata: Institution
      + Embedded Metadata: Copyright
  - Digital File Name
      + This only needs to be filled out if the collection holder has specified specific file names. For example Disney does specify their file names
      + For some collection holders this process can be fairly complicated because of how the collection holder provides their filename info:
      + Let's say that a collection holder provides a "File Name Root" to you. This is what they want the Preservation file called. However for us to process it we need to append out barcodes to the file name. The best way to do this is to combine the barcodes in the spreadsheet with the File Name Root as a calcuation.
      + Pase the File Name Root information from the collection holder into the first unused column (Column P)
      + You'll also need to have the barcode data in the spreadsheet already (Column C)
      + With that all set you'll need to set the Digital File Name column as the result of the following calculation in the second row (the first cell under the header)
      + Open Office: =CONCATENATE("BAVC";C2;"_";P2)
      + Excel: =CONCATENATE("BAVC",C2,"_",P2)
      + _Note: This assumes that Column C contains the Barcode, and Column P is the dummy column that contains the collection holder's provided filename_
      + You can drag down the corner to autofill this for every record.
      + Once you've autofilled everything, you'll need to copy the entire column, copy, then paste over it using Paste Special
      + Copy the entire column by selecting P (the button above the column)
      + Right click and select Copy
      + Navigate to Column H (Digital File Name), Right click in the first box and and select Paste Special
      + Uncheck every box under Selection except for Text (in other words, keep Text selected)
      + Click Ok
      + Rename the column to "Digital File Name" in case the name changed
      + You've now replaced the calculated value with just normal text. You can now delete the dummy column that contains the collection holder's provided filename
* Clean up the text
  - Sometimes there's text encoding issues that cause the text to be imported improperly. The most obvious times this happens is when there's these types of “quotes” instead of the standard "quotes". It's a huge pain, but make sure to go through and replace all quotes and apostrophes.
  - You can do this easily using Find and Replace. If you see any quotes or apostrophes in the text, copy them and past them into the "Find" window, and then type the non-weird version into the "Replace" window. Check out the following examples:
    + Find: “ Replace: "
    + Find: ” Replace: "
    + Find: ‘ Replace: '
    + Find: ’ Replace: '
* Save the file as a CSV

### Import The Data

Once you've created the CSV file that will be uploaded we'll use Data Import Wizard to upload the info into Salesforce.

* Open and log into Salesforce
* Click the Gear button at the top right and then press _Setup_
* On the left-hand side of the screen under _Integrations_ click _Data Import Wizard_
  + You can also just type "Data Import Wizard" into the _Quick Find_ search bar.
* Click Launch Wizard!
* Under What kind of data are you importing? select _Custom Object -> Preservation Object_
* Under What do you want to do? select _Add New Records_. Then under _Match_ by: select _Name_
* Drag the CSV file you've created where is says Drag CSV file here to upload under Where is your data located?
* Hit Next
* The next window will say Edit Field Mapping: Preservation Objects in this section you can make sure that all of the fields are mapped properly. If you notice anything is incorrect you can click Change and then type in the Salesforce field you want the selected CSV column mapped to.
* If everything looks correct hit Next
* If everything looks correct hit Start Import


### Cleanup

* You will receive an email when the import is completed. If there are any errors it will let you know.
* Common reasons for errors:
  - A required field was not filled in
  - The text in a field exceeds the maximum allowed characters
  - The rules of entry for a field were not properly met
* Note that imports do not like apostrophes and special characters, so they'll import as a question mark. The person completing intake will need to fix these.
* ![Data Import Errors]({{site.baseurl}}/assets/images/SalesforceImportError.png)


## Inventory Import
The steps for this process are very case-by-case depending on the data is observed in the collection holder's inventory. There is no one-size-fits-all approach, so don't think of these steps as anything other than guidance.

### Scenario: Mass Error Correction

This section is forthcoming!
