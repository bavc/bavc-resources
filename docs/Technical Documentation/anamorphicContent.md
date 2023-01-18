---
layout: page
title: Anamorphic Content
parent: Technical Documentation
---
# Dealing With Tapes Containing Anamorphic Content

## What Is Anamorphic Content?

Anamorphic content is widescreen content that is distorted (or squished) in order to fit within a Standard Definition 4:3 frame. The most common example is something like a 16:9 widescreen film that is squished to fit in a 4:3 frame. 

Before moving on, it's very important to note we're NOT talking about letterboxed (black bars on the top an bottom) or pillarboxed (black bars on the left and right) content. This is a different method of displaying 16:9 in a 4:3 frame (letterboxed), or 4:3 in a 16:9 frame (pillarboxed). Both of these methods leave a large amount of the frame unused. Anamorphic distortion avoids this.

The idea is that the content is shot in 16:9, squished to 4:3 when recorded to tape, then expanded to 16:9 when presented, so that the original aspect ratio is preserved. 

It may be easier to think about this in terms of film. Each frame of a film has close to a 4:3 width to height ratio. So how to they put widescreen content on film? They use an anamorphic lens (hence the term) that squishes the widescreen content to fit on the film. Then, when they project the film the image goes back through the lens, so that it stretches out to a proper widescreen image. 

The same concept is used in video, but instead of using a distorting lens, the shape of the pixels is changed. As you may know, most video content is 4:3. Analog video is 486 lines, and digitized SD video is 486x720 pixels. This never changes. So, in order to fit widescreen content into this SD frame, the original content is squished (distorted) before being printed to the tape. Then, when the tape is played back the pixels are stretched out before being viewed. On our fancy CRT monitors there's usually a 16:9 button that will stretch the image out. In a digital space, you'll need to change the aspect ratio of the digitized file to properly display the content. 

## Recognizing Anamorphic Content

It can sometimes be hard to tell if content is anamorphic or not, since the changes can be subtle. Here's some ways to help identify if the content is anamorphic:
* If people look tall, gaunt, or stretched out
* If familiar circular objects like balls, traffic lights or stop signs look distorted
* If subtitles look taller than usual

Anamorphic content can be found on almost any video tape format. However it is most common on MiniDV and DigiBeta. We don't need to worry about the workflow details for MiniDV because dvrescue takes care of it for us. 

## Capturing Anamorphic Content

At BAVC we prefer to preserve tapes as close to their original format as possible. As a result, digitizing anamorphic content is no different than digitizing non-anamorphic content. The Preservation Files that we deliver for anamorphic content will be distorted, 4:3 files. This is because we want to make sure there is consistency across all digitized files. 

However, you should note the fact that tape has anamorphic content in the Technicians Notes.

vrecord will display the content in 4:3, but you can set the CRT monitor to display the content as 16:9 if you want to make sure everything is looking good. The monitor will add black bars at the top and bottom, but these are not captured in the file (as long as the vrecord content looks the same)

## Creating Proper Anamorphic Access Files

While we don't capture anamorphic Preservation files at all, we do want to adjust the Access files so that they can be viewed in their proper aspect ratio. 

Our Transcode Engine script cannot handle anamorphic content (for now...), so you'll have to do it manually. There's two ways to do this:

### Method 1: Create a 16:9 Access File From the 4:3 Access File

This is perhaps the preferred method. You run the transcode engine script as normal, then reprocess the MP4 files to make them 16:9. This does not perform a transcode, but rather updates the file's "Aspect Ratio" metadata flag to 16:9 from 4:3. Basically, rather than changing the size of the file, it simply changes how the file is displayed. Just like project a film through an anamorphic lens!

You can run it on a single file like this: 

```
ffmpeg -i [Path To Input] -movflags faststart -aspect 16:9 -c copy  [Output File Path]
```

If you want to run a bunch if files in batch, you can `cd` into the folder you want to process and run this command:

```
for file in *.mov ; do ffmpeg -i "$file" -movflags faststart -aspect 16:9 -c copy "${file%.*}_access.mp4" ; done
```

### Method 2: Create a 16:9 Access File From the 4:3 Preservation File

The following string will create a 16:9 MP4 file from our standard Preservation Files. If you use this method do not use the Transcode Engine to create any MP4 files, instead just use this command:

```
fmpeg -i [input file]] -c:v libx264 -pix_fmt yuv420p -movflags faststart -crf 18 -b:a 160000 -ar 48000 -aspect 16:9 -s 640x480 -vf crop=720:480:0:4,yadif,setdar=16/9 [output file]
```

If you want to run a bunch if files in batch, you can `cd` into the folder you want to process and run this command:

```
for file in *.mov ; do ffmpeg -i "$file" -c:v libx264 -pix_fmt yuv420p -movflags faststart -crf 18 -b:a 160000 -ar 48000 -aspect 16:9 -s 640x480 -vf crop=720:480:0:4,yadif,setdar=16/9 "${file%.*}_access.mp4" ; done
```

