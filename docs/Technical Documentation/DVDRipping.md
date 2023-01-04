---
layout: page
title: DVD Ripping
parent: Technical Documentation
---

# Required Software

   * FFmpeg
      - install with brew install ffmpeg
   * ddrescue
      - install with brew install ddrescue


# Walkthrough with Details and Instructions

## Load the DVD Into the Drive
Either load the DVD into the slot on the side of the computer, or open the tray with the Eject (⏏) button on the keyboard, load the disc in the tray, then press Eject again to cose the tray

## Unmount the DVD using drutil
In order for ddrescue to rip the DVD, you'll need to unmount the disk from the computer. Unmounting is different from ejecting. Ejecting implies physically removing the disk from the computer. Unmounting simply means virtually removing the disk as a readable filesystem from the computer's directory

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
   0:                            PLAYBACK DVD            *8.0 GB     disk3
```

The DVD disk in this example is mapped to /dev/disk3. We can tell because it's 8.0GB, and the title is "Playback DVD". That just happens to be the title of this DVD. The title and device number WILL change on different computers and for different DVDs. You'll have to figure out what they are for each disk, just to be sure!

Now that you know the device path for the DVD, you can unmount it with the following command:

The general command is: `diskutil unmount /device/path`

You'll need to fill in the `/device/path` part yourself, so for this example it would be:

`diskutil unmount /dev/disk3`

The computer will respond with` Volume PLAYBACK on disk3 unmounted`

You're done with this step!

##Rip the DVD using ddrescue
Run the following command

`ddrescue -b 2048 -v /device/path /Path/to/output/folder/DiskName.iso /Path/to/output/folder/DiskName.iso.log`

The device path and paths to the outputs (DiskName.iso and DiskName.iso.log) will need to be filled out by you. This will create two files: An ISO, which is a clone of the disc but as a file, and a log of the process.

Continuing with the example from the previous step, this is the string we would use to rip the DVD to the Testing folder on the SAN

`ddrescue -b 2048 -v /dev/disk3 /Volumes/SymplyUltra/Testing/Playback_dvd.iso /Volumes/SymplyUltra/Testing/Playback_dvd.iso.log`

This command will take a while to run, depending on how long the DVD is and how damaged it is. ddrescue can recover damaged DVDs, but it takes a long time to run. The command will quit automatically once it is finished, and will let you know if any errors occurred.

#Run dvd_transcoder.py script on ISO
This script will automatically perform the following steps

   * Mount the ISO
   * Streamcopy the VOBS to a local directory
   * Create a list of VOBS to concatenate
   * Concatenate the VOBS and transcode them to a target format
   * Unmount the ISO
   * Delete temporary files

The script is located here, but you can drag it to your desktop for quick access: `/Volumes/Creative Services/Creative Services/PRESERVATION/Scripts/dvd_transcoder.py`

You can see the instructions for running the script if you run it with the -h flag, like so: 

```
dvd_transcoder.py -h

options:
  -h, --help        show this help message and exit
  -i I, --input I   the path to the input directory or files
  -f F, --format F  The output format (defaults to v210. Pick from v210,
                    ProRes, H.264, FFv1)
  -o O, --output O  the output file path (optional, defaults to the same as
                    the input)
  -m M, --mode M    Selects concatenation mode. 1 = Simple Bash Cat, 2 =
                    FFmpeg Cat
  -v, --verbose     run in verbose mode (including ffmpeg info)
```

As an example, here's how you would run it on an ISO to make a ProRes file:

```
dvd_transcoder.py -i /Path/To/InputFile.iso -f ProRes
```

And keeping with the previous example, here's how we would make a ProRes file of the Playback DVD example:

```
dvd_transcoder.py -i /Volumes/SymplyUltra/Testing/Playback_dvd.iso -f ProRes
```

It will take some time to run, but will you keep you updated on what it's doing. That's it!
