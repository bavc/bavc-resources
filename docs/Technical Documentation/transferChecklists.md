layout: page
title: Transfer Checklist
parent: Technical Documentation
---
{:toc}

# Transfer Checklists

## Transfer Setup Checklist

* Clean tape path and heads
* Test for color (in 1/2”) 
* Set H-position 
* Adjust tracking and skew
* Set chroma and hue
* Set luma 
* Set audio 
* Check digital audio levels in media express or audio scopes
* Check digital video levels in vrecord

## Post Transfer Checklist

* Check audio sync
* Using ffmpeg: trim beginning/end to be ~3 seconds on each end (see **trimmer.sh** script in Scripts folder on SAN)
* Delete original file
* Save new file as final file name
* Complete notes (make sure timestamped notes align correctly following trim)
* Complete Salesforce record
* Run **transcodeEngine.py**
* Mark Digitization Status: Pass

## Troubleshooting Checklist

- Were the heads and tape path cleaned thoroughly (try acetone)? 
- Was the tracking and/or skew adjusted (in 1/2” - try adjusting the tape guides)? 
- Is the correct transmission chosen in the capture card software? 
- Is the selected transmission set up properly (component, composite, etc)? 
- Does the problem occur on another deck?
- Does the problem occur on another TBC? 
- Is the TBC configured properly? 
- Are there settings or preferences in either the software or the computer that are causing the issue?
- Are you using the most up to date version of the software? 
- Have you baked and/or cleaned the tape? 
- Is the cassette damaged or rattling? 
- Have you tried re-housing the cassette? 
- Have you tried replacing a cable? 
- Is the bay patched correctly?
- For 1/2”, was the “color” selected for a black and white tape? 