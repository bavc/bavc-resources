---
layout: page
title: California Revealed Worklow
parent: Technical Documentation
---

# California Revealed Workflows

California Revealed has a number of deliverable specs that are slightly different tan our typical deliverables. There's nothing too serious, but there's enough changes to warrant an article on the subject. This article will describe in detail the specs and workflow considerations for working with California Revealed collections.

## Quick Background

California Revealed (CA-R) used to be the California Audiovisual Preservation Project (CAVPP), but then started accepted non-AV materials and changd their name. They are run out of the California State Library in Sacremento, CA. CA-R funds the digitization of collections with content pertaining to California History. Organizations are asked to apply, and if accepted they get free digitization. CA-R project manages the whole digitization project, and hands off the digitized and QC'd files to their partners. CA-R also hosts the digitized materials on [their webite](https://californiarevealed.org/). Check it out to see some amazing California-related content!

## Important Differences and Specs

To see the full list of CA-R's deliverable specs you can go to their website and read their [Statement of Work (SOW) for Audiovisual Recordings](https://californiarevealed.org/partners/sow). This [SOW]({{ site.baseurl}}/docs/Admin/projectStartup.html) contains all of the information you need to know about deliverable, timelines, client contacts, etc. Make sure to look over this before starting the project to be sure you're creating and delivering files correctly.

The SOW contains a file list of deliverable specs, but here we're just going to discuss the ones that are different than the usual BAVC stuff:

* *Preservation Files*
   - Format is FFV1.
   - Filenames formated as: [ObjectIdentifier]_prsv.[fileExtension]
      * Example: cusb_000001_prsv.mov

* *Access Copies*
   - 720x540
   - Filename always end in "_access.HD.mp4" (even through they're not HD)
   - Checksums: Sidecar MD5 files with format: "checkum *filename"
   - Descriptive metadata provided by CA-R should be instereted in the access file

* *Photos*
   - JPEG photos of tapes should be taken of all objects
   - No hard specs exist yet, besides that they are JPEG format.

## Workflow considerations

This section will describe the workflow steps you'll need to adjust or add to properly handle CA-R files

* Intake
   - Intake must be performed using the [Data Import Wizard]({{ site.baseurl}}/docs/Salesforce/dataImportWizard.html). This is because CA-R has metadata that they want embedded into their files. You need to make sure metadata from their spreadsheet gets imported into the following SalesForce fields so that it gets embedded in the files.
      * Embedded Metadata: Title
         - CA-R Metadata Field: "Audio Metadata: Title"
         - The title of the program
      * Embedded Metadata: Artist
         - CA-R Metadata Field: "Audio Metadata: Artist"
         - Only use if an Artist / Interviewee is included in the metadata
      * Embedded Metadata: Date
         - CA-R Metadata Field: "Audio Metadata: Date"
         - If there is no date, put "1/1/1900"
      * Embedded Metadata: Album
         - Only used for audio collections. Optional for video
      * Embedded Metadata: Comment
         - This should always just be "California Revealed"
      * Embedded Metadata: Institution
         - This should always just be the partner's name
      * Embedded Metadata: Copyright
         - This should be provided by CA-R, it is required
   - If any above the above required fields are provided by CA-R you should reach out to them before doing the import. Do not import until you have all of this settled.
   - You'll also want to import the filenames into SalesForce at this stage
      * This makes it easier for the trasnfer technician to avoid typos or forgetting the "_prsv" suffix
      * Import the filename into the SalesForce field named "Digital File Name".
      * This will eventually be over-written when the transcode engine harvests metadata from the file and puts in SalesForce. But it's useful to the transfer technician to have it before the transfer
      * Use a formala to concatenate the BAVC Barcode number with the identifier supplied by CA-R
      * The filename should be this format: [BAVC_Barcode]_[ObjectIdentifier]_prsv.[fileExtension]

* Ingest
   - Capture files as 10-Bit MOV. You'll transcode files to FFV1 later
   - Name the file as the following format: [BAVC_Barcode]_[ObjectIdentifier]_prsv.[fileExtension]
      * Example: BAVC1234567_cusb_000001_prsv.mov
      * *DO NOT FORGET* about the "_prsv" section. This is how the transcode engine knows how to handle the files as CA-R files. Without this you'll need to do tons of manual work.

* Transcode
   - Run the transcode engine
   - Select 2 Delivrables
   - Derivatives 1 will be _FFv1/MKV_ (option 3)
      * The script will ask "Do you want the MKV file to be the Preservation file?". Say Yes (option 3)
   - Derivatives 1 will be _H.264/MP4_ (option 1)
      * Select "De-interlace" (option 1)
      * Map the audio however is best for the file
      * For the frame size, select _720x540_ (option 3)
   - Save to PresRAID as normal
   - Create the QCTools report as normal
   - For the checksum select _Yes + Sidecar_ (option 2)
      * For the checksum format select _MD5 *filename_ (option 4)

* Post-Transcode Check
   - Using the example _BAVC1234567_cusb_000001_prsv.mov_ the following files should now exist:
      * BAVC1234567_cusb_000001_access.HD.mp4
      * BAVC1234567_cusb_000001_access.HD.mp4.md5
      * BAVC1234567_cusb_000001_prsv.mkv
      * BAVC1234567_cusb_000001_prsv.mkv.md5
      * BAVC1234567_cusb_000001_prsv.mov
   - Open up the checksum files with text editor and make sure they have the following format in the file
      * [checksum] *[filename without BAVC barcode]
      * Example: 986233f5d99de3b575b3907da3c7870a *cusb_000001_prsv.mkv
   - Don't mark the the file QC ready until it has photos too. It's best to take photos early on, either before or while transferring a batch so photos don't hold up QC.
   - If everything looks correct you can mark the tape QC ready and move it to the QC folder!
   - If the access filenames look incorrect, or the checksum format is incorrect (the BAVC barcode is in the checksum, or something similar) you probably misnamed the .mov file. Make sure _prsv.mov is in the filename of the script won't process the files properly.

* Quality Control
   - Most quality control aspects will be the same, but here's a few things that are important to check:
      * Preservation file is FFV1/MKV
      * Access copy has embedded title (which will show up Quicktime)
      * Access copy has embedded metadata, visible in Mediainfo (run Mediainfo on file)
      * Sidecar checksum files exist
      * Sidecar checksum has correct format
      * Photos exist and are of the correct object

* Loading
   - Files should be loaded to the drive as normal, this means Preservation and Access files are in their own folders. Checksum files should remain next to their associated files.
   - Make sure ALL files make it to the PresRAID before renaming or deleting files off the SAN
   - BAVC Barcodes need to be removed from the file before delivery
      * You can do this by hand for a small batch
      * For a large batch, check the renaming script in Scripts page. Be careful and make sure you 
