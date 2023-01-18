---
layout: page
title: Station and Equipment Qualification
parent: Technical Documentation
---

# Station and Equipment Qualification

Qualification refers to making sure that equipment is working as expected, and not introducing errors or problems. Equipment should be tested and qualified when received to make sure it's ready for use. Transfer stations should be qualified if anything is changed, such an OS update, hardware update, or equipment is switched out.

Morgan created a workshop focused on station qualification that can be [accessed on GitHub](https://github.com/iamdamosuzuki/AV-Pres-Validation-Workflows). This workshop discusses all the major concepts behind station qualification, and has exercises about how to qualify and test stations. If you have time you should work through all the exercises to be sure you understand the details of station qualification.

## Video Transfer Station Qualification

As mentioned earlier, any time changes are made to a computer or station it should be re-qualified. This includes OS updates, hardware updates, software updates, etc. We've experienced situations where certain BlackMagic firmware updates causes the BlackMagic cards to clip video signals at broadcast range. This is why it's important to test and qualify stations when changes are made.

Here's a list of key things you'll want to test for:

* Is the video signal clipped at all? Can the full video range be properly captured?
   - Use QCTools Waveform Scope to check
* Is the transfer station capturing a full 10-Bit signal?
   - Use QCTools Bitplane and Waveform Scope to check this
* Is there any unwanted noise in the audio?
   - Use FFmpeg to generate a spectrogram to check this
* Are there any dropped frames in the audio or video?
   - Use QCTools VREP to check this for video
   - Use FFmpeg to generate a spectrogram to check this for audio

## Audio Transfer Station Qualification

Generally audio stations are qualified using a "Null Test." A null test is when a signal is recorded on a known good station, and a test station. The two signals are then nulled (subtracted from each other, or added together with one of the signals out of phase) If the output of the null is completely blank then you know that that signals were identical. This is a way to make sure that there were no dropouts, level issues, or other errors introduced during recording.

When we first got out MOTU UltraLite-mk4 we noticed that there were dropouts occurring constantly when recording at 96kHz. We eventually fixed this by upgrading the firmware and the computer's OS. However, to test it and make sure there were no dropouts happening at all we wanted to make sure we could record a signal without any dropouts. This is what we did:

* First, used FFmpeg to create a one our long 96kHz/24bit Sine Wave
   - `ffmpeg -f lavfi -i "sine=frequency=96000:duration=3600" -c:a pcm_s24le 96khz_Sine_Wave.wav`
* Created a 96kHz / 24bit Project in reaper
* Drag the 96kHz sine wave into track 1 in the project
* Set the output of track 1 in the routing to go to Analog Out 1 & 2
* Use two balanced patch cables to send Analog Out 1 & 2 to Analog In 1 & 2
* Create a new track, set them to receive Analog In 1 & 2
* Set all the trim values to unity
* Hit play and record, ideally recording the sine wave coming out of the box straight back into the inbox
* Null the two waves
* The output should be zero, or extremely close to zero! If there are spikes or chunks of audio that phase in and out then the box is introducing errors and dropped samples 
