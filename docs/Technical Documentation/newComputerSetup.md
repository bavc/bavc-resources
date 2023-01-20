---
layout: page
title: New Computer Setup
parent: Technical Documentation
---
# Setting Up A New Transfer Station Computer

## Connecting SAN

- Make sure that Fibre is connected and that a second Ethernet Connection (likely USB3 to Ethernet) is connected to the metadata network
- in the NewStationSoftware on Google Drive folder run the program called "SymplyUltra.configprofile". This will configure the computer to be attached to the SAN
- in terminal run "sudo xsanctl mount SymplyUltra"

## Prepping Computer

- Turn of SIP by booting into Recover Mode, opening terminal, and running "csrutil disable"

## Installing Software

- Install the latest version of Blackmagic Desktop Video that the OS you're on is compatible with
  - Desktop Video 12.4 is the latest version that MacOS 11 BigSur can handle
  - After installing Desktop Video you'll need to allow security extensions. You can do this by going to System Preferences -> Security & Privacy, then unlocking the window and pressing "Allow" at the bottom of the screen.
- Install all the software in the NewStationSoftware folder on Google Drive
- Install the latest version of XCode compatible with the OS version running on the computer. You can find [XCode installers here](https://xcodereleases.com/).
- Install Homebrew using the directions here: [https://brew.sh/](https://brew.sh/)
- Install the critical command tools via homebrew with these commands:
  * brew tap amiaopensource/amiaos
  * brew install vrecord
  * brew install ffmpeg
  * brew install qcli
  * brew install mediainfo
  * brew install sox
  * brew install id3v2
  * brew install cdrtools
  * brew install cuetools
  * brew install bwfmetaedit
  * or, you should be able to run `brew tap amiaopensource/amiaos && brew install vrecord ffmpeg qcli mediainfo sox id3v2 cdrtools cuetools bwfmetaedit`
