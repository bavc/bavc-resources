---
layout: page
title: CD Ripping
parent: Technical Documentation
---

# CD Ripping

There are generally 2 types of CDs: CD-ROM and CD-DA. CD-ROMs contain an imaged volume with data, and can generally be treated like DVDs but with slightly different ripping parameters. CD-DAs contain just audio, and need to be ripped with special software.

## Ripping CD-DA discs

At BAVC we have created specific workflow for ripping CD-DAs. Our preferred method is to use XLD to rip the CD to a single WAV file. Once the file is ripped, it is possible to either create just a single MP3 access file per disc, or to split the MP3 access files according to the original tracklist on the CD.

The reason we rip the CD as a single WAV file is because it's easier to manage a single file. Our system has trouble keeping track of multiple files in a single record, especially if the number of files is variable like it often is with CDs. However, since our automation embeds the tracklist into the file, the original tracklist can always be extracted from the single WAV file.

## Required Software

* XLD
   * You can install by either:
      * Installing the DMG file from [this site](http://sourceforge.net/projects/xld/files/xld-20220917.dmg)
      * If you have homebrew install you can run the command `brew install xld`


## How To Rip
* Create folder with unique file name (BAVCbarcode_client ID)
* Open XLD, set output directory to new folder
* Under file name, set to custom and paste in folder name
* Under CD Rip, set ripping mode to CDParanoia III 10.2
* Click the following settings
   * REQUIRED
      * **Ripper Mode**: CDParanoia III 10.2
      * **Read samples offset correction value**: 0
         * ✅ Set automatically if possible.
      * **Save Log File**: Always
      * **Save Cue File**: Always
   * OPTIONAL
      * **Max retry count**: 20
         * You can set this to more or less if you’d like.
      * **Drive Speed Control**: Automatic
      * ✅ Query AccurateRip database to check integrity
      * ✅ Automatically open disc upon insertion
      * All other fields are optional, set them as you'd like
   * You can use this image for reference:
      * ![XLD Options]({{site.baseurl}}/assets/images/XLD-Options.png)
   * Insert CD
   * Second window will pop up. Make sure option in top left says *Save as a single file (+ cue)*
      * ![XLD Ripping]({{site.baseurl}}/assets/images/XLD-ripping.png)
   * Make sure you rip as a single file!


## During Rip
* Barcode Case
* Do NOT barcode CD
* Update Intake details
* Update as much of the following metadata fields as possible. Any of these fields can be left blank if they are unknown or unapplicable
   * **Embedded Metadata: Title**
      * Label on the tape
      * Or, if the title is clearly a subset of the label just put the title
   * **Embedded Metadata: Artist**
      * Music or art = Main artist or musical group
      * Oral history or interview = Interviewee’s name
   * **Embedded Metadata: Date**
      * Year only = 1/1/YYYY
      * Year and month  = MM/1/YYYY
      * Year months and day = MM/DD/YYYY
      * Unknown = leave blank
   * **Embedded Metadata: Album**
      * If it’s actually an album then use the album name
      * Otherwise, use the collection name
   * **Embedded Metadata: Institution**
      * Use the collection holder’s institutional name
   * **Embedded Metadata: Comment**
      * Optional field, but could include any errors encountered during transfer or any other salient information
      * Mention if the tracklist noted in liner notes or on the container differs from the cue file tracklist.
      * Essentially, any information from the Technicians Notes field could go here, but it is optional
   * **Embedded Metadata: Copyright**
      * If the collection holder has mention specific copyright information it should be included here. This is mostly used by California evealed
   * **Technicians Notes**
      * Any errors encountered during transfer or any other salient information
      * Mention if the tracklist noted in liner notes or on the container differs from the cue file tracklist.

## After Rip
* Rename log file to match ripped wav file name, but with .log extension
* Check log file for track number, any errors
   * If there are errors, Mark file as review in Salesforce and check the file’s spectrogram for an obvious errors
   * If no errors are audible in the file or clearly visible in the spectrogram you should note any errors mentioned in the log in the Technicians Note, mark the file as pass and move on.
* Run the [`cdEngine.py`](https://github.com/bavc/videomachine/blob/master/cdEngine.py) script on a folder containing all folders that need to be processed. Check to ensure metadata is uploaded on Salesforce
   * Run the script with the following command to create a single MP3 from the rippped WAV file:
      - `cdEngine.py -i /Path/To/Folder`
   * Run the script with the following command to create an MP3 for every track on the original CD file:
      - `cdEngine.py -i /Path/To/Folder -s`
   * Run the script with the following command to create an MP3 for every track on the original CD file, along with a single MP3 containing all of the CD content:
      - `cdEngine.py -i /Path/To/Folder -s -m`
   * It is also possible to run the `cdEngine.py` script on a single folder containing the files for a single CD if you’d like.

## Quality Control

### Bulk Quality Control
Since CDs are often transferred in large volumes, it’s best to check specifics about the metadata in bulk using BWFMetaEdit.

* Open BWFMetaEdit
* Drag the folder containing all the CD subfolders into BWFMetaEdit.
* Technical Metadata Check
   * Click on the Tech tab
   * Check that every file meets the following specs:
      * **Format**: Wave
      * **CodecID**: 0001
      * **Channels**: 2
      * **SampleRate**: 44100
      * **BitRate**: 1411200
      * **BitPerSample**: 16
      * *Note: The specifications for CDs are very strict. If any of these specs don’t match then the file wasn’t properly ripped from the CD*
* Descriptive Metadata Check
   * Click on the Core tab
   * Make sure that the embedded descriptive metadata for every file makes sense and has been properly filled out.
   * Here are the fields that are most important to check
      * **Description**
         * Should include the Title and the date. Make sure it doesn’t include the date twice by accident, which is a common way for this to fail based on how the script works.
      * **Originator**
         * Should be “BAVC”
      * **OriginationDate**
         * Should be the date the file was ripped, so within a few days of QC
      * **BextVersion**
         * Should be “1”
      * **CodingHistory**
         * Should contain the entire log file. If it’s empty there is an error
      * **ICRD**
         * Should be the original recording date of the CD (if it exists)

### File by file Quality Control
For the most part these files can be QC’d in bulk, but there are a few things that need to be checked individually.

* File playback
   * Preview the file quickly to make sure it plays. No need to check heads and tails because we do not trim these manually
   * If any files have been flagged for having a high number of errors during the rip they should be played back to ensure that these errors are not present during playback. If the errors are present then the disc should either be retransferred, or the errors should be noted according to the technician’s discretion.
* MP3 Metadata
   * Opening the MP3 in iTunes will show the file’s embedded metadata. Make sure that it looks correct compared to the metadata in the Salesforce record
   * If MP3 is split, check that the number of MP3 files matches the tracklist in the CUE and LOG files.

### Bulk QC Updates in Salesforce
Because CDs often come in large numbers, it’s easier to update the QC records for them in bulk using Views in Preservation.

* Open the Preservation Object View named !(CD QC)[https://bavc.lightning.force.com/lightning/o/Preservation_Object__c/list?filterName=00B2J000009e0vzUAA]
* This view will show any CD objects that have been marked *Digitization Status:* ***Pass***, but have not been marked *Quality Control Status:* ***Pass***
* In order for these records to be considered a QC Pass, the following fields visible in this view need to be filled out. None of these fields can be empty:
   * **Digital File Name**
      * This should reflect the file name without the extension
   * **Digital File Duration**
      * This should reflect the file duration in format HH:MM:SS:mmm
   * **Digital Audio Sampling Rate**
      * This should always be 44100
   * **Checksum**
      * This should have checksum in it.
   * **Source Deck**
      * This should reflect the disc drive used to rip the CD
      * If the drive is internal to a computer is should say “Internal”
   * **Host Computer**
      * This should reflect the computer that was used to rip the CD
* If the above fields are properly filled, you will need to update the following fields. These fields can be easily updated in bulk by clicking the checkbox on the far left side of the list for every record you want to update. Then, when you update each field make sure to click the box that says “Update X selected items” (where “X” is how many records you’ve selected)
   * **Quality Control Date**
      * The date that you are performing QC
   * **Quality Control Notes**
      * This should only be filled out if there is a problem. The format should be:
      * QC User Initials - Date - Note
   * **Quality Control User**
      * Your username
   * **Metadata**
      * “Pass”
      * This is the only “QC Checklist” record we’re updating unless you’re performing full single-file QC
   * **Quality Control Status**
      * “Pass”
      * This is the most important field, since it determines the QC status of the record.

### Ripping CD-DAs Outside of BAVC

If you plan to rip CDs at an institution outside of BAVC you can follow most of the main steps. You'll still want to use XLD, but you won't be using Salesforce at all to handle the metadata. Instead of using `cdEngine.py`, which by design needs to be connected to salesforce, you can use [`simple_cd.py`](https://github.com/bavc/videomachine/blob/master/simple_cd.py), which works the same way, but allows the user to manually enter any embedded metadata.

* Run the script with the following command to create a single MP3 from the rippped WAV file:
   - `simple_cd.py -i /Path/To/Folder`
* Run the script with the following command to create an MP3 for every track on the original CD file:
   - `simple_cd.py -i /Path/To/Folder -s`
* Run the script with the following command to create an MP3 for every track on the original CD file, along with a single MP3 containing all of the CD content:
   - `simple_cd.py -i /Path/To/Folder -s -m`

## Ripping CD-ROM discs

CD-ROMs are different than CD-DAs, and have to be treated differently. Since CD-ROMs can contain any arbitrary file-based data, rather than the strict audio configuration that CD-DA's always have, we need to approach the migration by creating an ISO image of the CD-ROM. If you're familiar with ripping DVDs you'll see that this is very similar to how DVDs are handled. You can see how to handle DVDs int the article titled [DVD Ripping](https://bavc.github.io/bavc-resources/docs/Technical%20Documentation/DVDRipping.html).

## Required Software

* ddrescue
   * If you have Homebrew installed you can install by running the command `brew install ddrescue`

## Load the CD Into the Drive
Either load the CD into the slot on the side of the computer, or open the tray with the Eject (⏏) button on the keyboard, load the disc in the tray, then press Eject again to cose the tray

## Unmount the CD using drutil
In order for ddrescue to rip the CD, you'll need to unmount the disk from the computer. Unmounting is different from ejecting. Ejecting implies physically removing the disk from the computer. Unmounting simply means virtually removing the disk as a readable filesystem from the computer's directory

First, you need to find out the Device Path of the disc you just loaded into the computer. To do this, you need to run the diskutil list command. If you're on a SAN client, or a computer with a bunch of drives connected, this will give you big long list of connected disks. You'll need to find the one that corresponds with the DVD you're loaded. Here's a sample:

```
Prez5-mp:~ preservation$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *3.0 TB     disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                  Apple_HFS Media2_3TB              3.0 TB     disk0s2
/dev/disk1 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk1
   1:                        EFI EFI                     209.7 MB   disk1s1
   2:                  Apple_HFS Mac HD                  499.2 GB   disk1s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk1s3
/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *3.0 TB     disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:                  Apple_HFS 10.11.6_BU              3.0 TB     disk2s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk2s3
/dev/disk3 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                            PLAYBACK CD            *700.0 MB     disk3
```

The DVD disk in this example is mapped to /dev/disk3. We can tell because it's 700.0MB, and the title is "Playback CD". That just happens to be the title of this CD. The title and device number WILL change on different computers and for different CDs. You'll have to figure out what they are for each disk, just to be sure!

Now that you know the device path for the DVD, you can unmount it with the following command:

The general command is: `diskutil unmount /device/path`

You'll need to fill in the `/device/path` part yourself, so for this example it would be:

`diskutil unmount /dev/disk3`

The computer will respond with` Volume PLAYBACK on disk3 unmounted`

You're done with this step!

##Rip the CD using ddrescue
Run the following command

`ddrescue -b 2048 -v /device/path /Path/to/output/folder/DiskName.iso /Path/to/output/folder/DiskName.iso.log`

The device path and paths to the outputs (DiskName.iso and DiskName.iso.log) will need to be filled out by you. This will create two files: An ISO, which is a clone of the disc but as a file, and a log of the process.

Continuing with the example from the previous step, this is the string we would use to rip the CD to the Testing folder on the SAN

`ddrescue -b 2048 -v /dev/disk3 /Volumes/SymplyUltra/Testing/Playback_cd.iso /Volumes/SymplyUltra/Testing/Playback_cd.iso.log`

This command will take a while to run, depending on how long the CD is and how damaged it is. ddrescue can recover damaged CDs, but it takes a long time to run. The command will quit automatically once it is finished, and will let you know if any errors occurred.

##Post-Rip Processing
Once the CD has been ripped you'll be left with an ISO file. This file contains the exact same data as the CD, with the same structure and organization as the original CD. This file should be considered the "Preservation File". It may be difficult to create "Access" files from this, depending on what the contents of the CD are. There may be hundres of folders, sub-folders, and files. Or they may just be a single video or audio file. Discuss with the client how they would like the concept of "Access" handled in this case. However, stress to the client that the ISO is the preservation file. If the ISO file is properly preserved, the CD is no longer needed. 
