# Easy-as-pie Android Decompiler

## Why
### One stop shop
I got pretty tired of decompiling Android apps with a bunch of steps that I had to remember all the time. It involved a lot of apktool, dex2jar, and jd-gui; it still confuses me.

### Collocation of source files
Further, even after these steps were complete (usually a combination of dex2jar and JD-GUI), I would be left with disparate sources of information; the decompiled Java would be over here in this directory, while the un-DEXed content would be somewhere else (Really bad for importing into eclipse!)

I basically wanted to make this generate a tree and source as close as possible to what the original Android developer sees.

### Regeneration of R.* References
One thing that existing decompilers *don't* do is regenerate R references; this tool includes a script that makes an attempt to do this. Which gives you more insight when you're reading source code?  
`View v = inflater.inflate(217994357, container, false);`  
or  
`View v = inflater.inflate(R.layout.result_panel, container, false);`

Now you can easily see and search for what resource is doing what, without needing to file-search R.java for some opaque int.

**Note:** This process relies on guesses and may lead to weird results, because the resource ints were inlined and opaque. You can check out the source code of rreassoc.py to see my matching heuristics and adjust them appropriately.

## What
apk2gold is a collection of (excellent!) 3rd-party tools and original content (the RRegenerator). It is designed to be easily installed and to get you the best results for Android app introspection as quickly as possible. The project stands on the shoulders of the following giants:

* **[nviennot/jd-core-java](https://github.com/nviennot/jd-core-java)** (or technically my [fork](https://github.com/lxdvs/jd-core-java) thereof, which actually builds under OSX ;) and by extension, **[JD](http://java.decompiler.free.fr/)**

* **[dex2jar](http://code.google.com/p/dex2jar/)**

* **[apktool](http://code.google.com/p/android-apktool/)**

## Installation

### Dependencies

You'll need python, git (for submodules), and mercurial (hg) for the sub-builds. Sorry!

### Installing
Just run make.sh after you make it executable (chmod +x make.sh).

## Usage

### Getting the APK 
There are different ways to acquire an APK, but the easiest is to just download it from the Play Store and use ES File Explorer to back up the APK (ES File Explorer -> "AppMgr" tab -> long click on app you want -> backup). The APK is now in the 'backups' directory on your SD card. Now you can just USB it over (I like to email it to myself from ES File Explorer itself). More depth can be found at [this SO post](http://stackoverflow.com/questions/12175904/where-can-i-find-the-apk-file-on-my-device-when-i-download-any-app-and-install).

### Decompiling
Actually using my tool easy as pie! Just use:  
`apk2gold <target>.apk`

### Looking at the result
This will create a folder with the APK's name in the same directory. Everything is in there. There is also an additional directory you may not recognize, `/.smali`, which contains the Smali output from APKTool. It's just kept around for reference, in case JD did something bad. Load it up in Eclipse and have fun!

## You know what would be cool?
It would be real cool to look for sections that JD decompiled incorrectly and sub in the Smali code generated by apktool. That would be baller.