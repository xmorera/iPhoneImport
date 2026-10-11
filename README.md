# iPhoneImport

This Python script copies files from an iPhone (or, well, any other device that is accessible from the Windows Shell) to
a destination folder on Windows.

The list of files that have been copied can be logged into a metadata folder so that next time the script is run it can
tell which files are new.

The script uses Windows Shell operations to copy the files.

## Why I wrote this script

I back my photos on my phone up regularly to my Windows PC, I wanted to have a one click solution to do so.

## Prerequisites

To be able to use this script, please perform the following steps:

   1. Install Python 3.10 from the Microsoft Store 
   2. Install PyWin32 using the following command: 
 
    pip install pywin32

## How to use

First, copy this project into a folder on your machine, e.g. C:\Scripts\iPhoneImport. Then run one of the following
commands:

Copy **all files** from the phone to a folder:

    python C:\Scripts\iPhoneImport "This PC\Apple iPhone\Internal Storage" "C:\ImportPixs"

Copy **new** files from the phone to a folder (i.e. skip the ones already copied):

    python C:\Scripts\iPhoneImport "This PC\Apple iPhone\Internal Storage" "C:\Pictures\iPhone\import" --metadata-folder d:\Pictures\iPhone\import\metadata

**Dry-run** mode: skip copying, write metadata only

    python C:\Scripts\iPhoneImport "This PC\Apple iPhone\Internal Storage" "C:\Pictures\iPhone\import" --metadata-folder d:\Pictures\iPhone\import\metadata --skip-copy

## Picking this up on another machine

Windows only (it uses the Windows Shell COM APIs).

    git clone https://github.com/xmorera/iPhoneImport.git
    cd iPhoneImport
    python -m venv iivenv
    iivenv\Scripts\activate
    pip install -r requirements.txt

Already cloned? Just update it:

    git pull origin main

Then plug in the iPhone, unlock it and tap "Trust", and run the script as described in "How to use" above. The metadata
folder is not in git, so point `--metadata-folder` at the same synced location (e.g. a Dropbox folder) on every machine,
otherwise files will be imported again.
