---
layout: page
title: Performing QC on DCPs
parent: Technical Documentation
---

# Performing QC on DCPs

## Overview

Digital Cinema Packages (DCP) are files specifically made to be projected in a Digital Cinema Projector. They have a very specific structure and format, and thus it's very important to QC them before they are projected for a festival, screening, or exhibit.

QCing DCPs involves using some specialized software, along with some QC steps that are typical of most BAVC QC Workflows. This article will describe the general process of QCing DCPs, as well as how to install the software. Installing the software is *not easy at all* so be very careful and read the instructions thoroughly


For the purpose of testing the QC software you can use [this](https://www.charbon-studio.com/get/dcps/CHARBON-INTEROP-24.zip) sample DCP, which is found in this [repository of sample files](https://www.charbon-studio.com/resources).

## Installing QC software

The main software tools you'll use to QC a DCP are `dcp-o-matic` and `clairmeta`. `dcp-o-matic` will playback DCPS, so it's an important tool for viewing the content. `clairmeta` can perform checks on fixity, structure, naming, etc, so it's very useful to make sure that the files and directory are properly formed.  It's easy enough to install `dcp-o-matic` using `homebrew` with the following command:

```
brew install dcp-o-matic
```

Ok that's easy enough. Let's dive into the hard part

### Installing clairmeta

There is no easy way to instal clairmeta, or some of the dependencies, so you have to do a few thing the hard way. [You can find the GitHub page for it here.](https://github.com/Ymagis/ClairMeta) First, lets discuss the dependencies:

* asdcplib
* mediainfo
* sox

`mediainfo` and `sox` can be easily installed using homebrew. Do that first, but there's a good chance the computer your own already has those programs installed.

The hard part is installed `asdcplib`. In order to do that you'll need to download the github repo for `asdcplib` which can be [found here](https://github.com/cinecert/asdcplib). Once you've done that, use `cd` to change the working directory of terminal to the root level of the download github repo (to be safe, you can move the github repo to whatever folder all your github repos live in first).

Once you're in the `asdcplib` directory run the following commands

```
autoreconf -if
./configure --enable-as-02
make
make install
```

If done correctly, a bunch of new command line tool will have been installed. You can test this by running the `asdcp-test` command. You should see output like

```
No operation selected (use one of -[gGcitux] or -h for help).
There was a problem. Type asdcp-test -h for help.
```

This means the tool was installed (though you ran it incorrectly as it says, but that's ok for now we just want to see if it installed). If you see something like this:

```
zsh: command not found: asdcp-test
```

Then you didn't install the tool properly. Check the commands you ran and try again! You may need to use `sudo` but it's not likely or necessary.

Now, it's time to install `clairmeta` itself.

go to the [Github Repo](https://github.com/Ymagis/ClairMeta) and download it. Move the GitHub repo to where ever you like to keep your repos. Move your terminal window into the repo by using the `cd` command.

Now you can install the tool by running the following command:

`python3 ./setup.py install`

Once this is finished, you can test that the tool has been properly installed by running the following command:

```
python3 -m clairmeta.cli
```

If it ran properly you should see the following output

```
usage: cli.py [-h] {check,probe} ...

Clairmeta Command Line Interface 1.3.0

positional arguments:
  {check,probe}
    check        Package validation
    probe        Package metadata extraction

options:
  -h, --help     show this help message and exit
```

If this looks correct then congrats! You've got clairmeta up and running. 

## Running QC software

### Running clairmeta

Clairmeta lets you probe and check DCPs. This is the standard command for probing a DCP:

```
python3 -m clairmeta.cli probe -type dcp [path/to/dcp]
```

This will just spit out a bunch of metadata in a json format. You can output this to a file using the following command

```
python3 -m clairmeta.cli probe -type dcp [path/to/dcp] -format json > [output/path.json]
```

In that case you need to define the output path and give it a .json extension. You can really make it anything, just be sure it ends in .json, though it's easiest if you just make it a sidecar file of the original DCP *(don't put it in the DCP!)*

To check the DCP you'll use the following command:

```
python3 -m clairmeta.cli -check dcp [path/to/dcp]
```
