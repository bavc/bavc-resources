---
layout: page
title: Removing Barcodes
parent: Technical Documentation
---
{:toc}

# Removing Barcodes using the Rename Command

## Rename: General Use

Rename is a super useful tool for renaming files and folders in bulk. It's pretty tricky to use, but here's a quick explanation of a simple use case

  * Installation is simple, just run `brew install rename`
  * In this example we will rename all files with the     extension .jpeg to have the extension .jpg instead.
  * First you need to `cd` into a the folder you want to rename, like this:
    - `cd Path/To/Directory`
  * Then run this command
  ```
  rename -n 's/\.jpeg/\.jpg/' *
  ```
  * The `-n` flag will run in dry-run mode,  meaning it will tell you what it will do before doing it.
  * `s/\.jpeg/\.jpg/` tells rename to look at the standard in, and replace ".jpeg" with ".jpg" in any file it finds
  * the `*` tells rename to look at every file in the working directory.
  * Once you run the command in dry-run mode it'll show you what changes it will make. If you're happy with that run the command again without the -n flag and it'll actually rename the files:
  ```
  rename 's/\.jpeg/\.jpg/' *
  ```
The above example just runs on files in a single folder. If you want to run the command on a bunch of recursive directories you'll need to use a more complex command:
  * First you need to `cd` into a the folder you want to rename.
  * Then run this command
  ```
  find . -type f -name '*.jpeg' -print0 | xargs -0 rename -n 's/\.jpeg/\.jpg/
  ```
  * This part: `find . -type f -name '*.jpeg' -print0 | xargs -0` performs a find in the working directory for any file ending with ".jpeg". Then it passes the file path to rename, which runs in dry-run mode
  * If you're happy with what dry-run mode comes up with, you can run it for real without the -n flag
  ```
  find . -type f -name '*.jpeg' -print0 | xargs -0 rename 's/\.jpeg/\.jpg/
  ```
In both examples we're just replacing .jpeg with .jpeg with this command: `s/\.jpeg/\.jpg/`

Keep in mind you can replace any string with any other string. It's very useful!

## Removing Barcodes

`rename` can very easily remove barcodes from files names. However, it's **very dangerous** because you can easily mangle files names with this command.

  * First, you need to make sure you remove all hidden files from the working directory.
  * `cd` into whatever directory you want to rename the files in
  * From here you can run the following command to remove the barcodes (first 12 characters) from every file in the folder
  ```
  rename -n 's/.{12}(.*)/$1/' *
  ```
  * That command actually runs in dry-run mode. Remove the -n to run it for real if you're happy with the projected results of the dry-run
  * Make sure you don't run this command twice! It doesn't know to remove barcodes, it just knows to remove the first twelve characters in a filename, so be very careful

  You can read the manual [here](http://plasmasturm.org/code/rename/)
