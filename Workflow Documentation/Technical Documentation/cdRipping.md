# CD Ripping

There are generally 2 types of CDs: CD-ROM and CD-DA. CD-ROMs contain an imaged volume with data, and can generally be treated like DVDs but with slightly different ripping parameters. CD-DAs contain just audio, and need to be ripped with special software. This article will explain how to rip CD-DA discs specifically.

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
      * Ripper Mode: CDParanoia III 10.2
      * Read samples offset correction value: 0
      * ✅ Set automatically if possible.
      * Save Log File: Always
      * Save Cue File: Always
   * OPTIONAL
      * Max retry count: 20
      * You can set this to more or less if you’d like.
      * Drive Speed Control: Automatic
      * ✅ Query AccurateRip database to check integrity
      * ✅ Automatically open disc upon insertion
      * All other fields are optional, set them as you'd like
   * You can use this image for reference:
      * ![XLD Options](/assets/images/XLD-Options.png)


This page is mostly incomplete, and is just a landing page for research on the topic.
