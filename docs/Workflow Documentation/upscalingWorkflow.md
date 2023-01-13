---
layout: page
title: Upscaling Workflow
parent: Technical Documentation
---
{:toc}

# Upscaling Workflow

## Required Software

* Black Magic Desktop Video Setup
* Black Magic Media Express

## Required Hardware

* Upscale Laptop
    * Apple MacBook Pro with Thunderbolt 2 connectivity
* CaptureRAID
* Teranex2D

## Setting up Hardware and Software

When we upscale a tape we are actually performing two captures at once. One of the capture is performed using the typical Pres Grotto computer and vrecord, to our typical Preservation Specs (10-bit Uncompressed MOV or FFV1/MKV). The second capture is performed using the Upscale Laptop and the Teranex2D. On this system we'll capture a ProRes422 HQ file that has been upscaled to 1080p.

* Setting up Upscale Laptop
    * This laptop should be on an extendable shelf in the Pres Suite video rack.
    * You will need to plug it into the Teranex via a Thunderbolt 2 cable
    * You will need to plug in a CaptureRAID drive via USB3
        * Once you have the CaptureRAID mounted, create a project folder on the drive and create the following folders
            * 01Ingest
            * 02ToSAN
            * 03Delete
* Setting up Teranex
    * The Teranex is the device that will perform the upscaling and act as the capture device for actually capturing the upscaled video.
    * **Ensure that the computer reads the Teranex**
        * Once the Teranex and the computer are conncted, open up *Black Magic Desktop Video Setup*
        * This software should say that the Teranex2D is a connected device. If not, make sure the Thunderbolt cable is properly connected.
    * **Get Signal to the Teranex**
        * The Teranex will need to receive a proper SDI signal. Ideally you will send the SDI signal from TBC1 (DPS-575) to the Teranex through the Patch Bay
        * You will can then send the original Standard Def SDI signal to the Pres Suite computer from the Teranex SDI Loop out in the patch bay. This is the signal that the Pres Suite computer will capture as a Preservation Master
        * If you have done this properly you should be able see the input video on the Teranex front panel
* Setting up *Black Magic Media Express*
    * Once you have the connected the Teranex and have video going to it, you can open *Black Magic Media Express.*
    * Once open, you should click the *Log and Capture* tab, you should see the video being sent to the Teranex.
    * Click *Media Express -> Preferences*
        * Under *Project Video Format* select the 1080p and the proper frame rate. You can find the desired framerate in the SalesForce Opportunity record
        * Under *Capture File Format* select Quicktime Apple ProRes 422 HQ
        * Under *Capture Audio and Video To* select the 01Ingest folder in the Project Folder
        * Exit by clicking the red x. This will save the settings
    * Name the file with the following filename structure
        * BAVC[Barcode#]_[ClientID]_mezzanine_take01
        * You can use the different fields available to build the file name, and clicking the plus-sign next to the field. The underscores will be added automatically
    * Record enable the desired audio channels
        * This is **VERY IMPORTANT!** Only the audio channels that are record-enabled will be recorded, even if the levels are metering. Make sure you record the audio properly.
            * Capture 2 channels if there are two or less channels
            * Capture 4 channels if there are 3 or 4 channels
            * Capture 8 channels if there are 5 to 8 channels
            * Capture 8 channels if there are 5 to 8 channels
            * Capture 16 channels if there are 9 to 16 channels

## Upscale Workflow

* One the hardware and software are setup you're ready to capture!
* **Capturing The Files**
    * Try your best to start vrecord and Media Express on the two different computers at the same time
    * To capture in Media Express press the "Capture" Button. IT will turn red while capturing
* Post-capture Workflows
    * Syncing files to SAN
        * The Preservation files will likely already be recorded to the SAN, which is normal. No need to do anything here
        * The Upscaled Mezzanine file will need to be sync'd to the SAN.
            * Plug the CaptureRAID to a SAN-connected computer and rsync the files to the 02InProgress folder of the project on the SAN