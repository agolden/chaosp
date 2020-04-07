# Customized Hybrid AOSP (CHAOSP)  

## Introduction  

This project aims to help you build [RattlesnakeOS](https://github.com/RattlesnakeOS), an AOSP ROM targeting the (Google) devices actually supported by AOSP, from Android 9 Pie onwards, locally, without using the AWS stack.  
It's a mainly privacy/security focused ROM (no Google Play Services neither root by default).  
Regarding this project name, and since we all like having a fully working smartphone with push notifications and so, it's also possible to bake in some patches before building the ROM.  
At the end, you'll get a flashable ROM customized to your need.  


## Features  

* pure AOSP  
* locally built (no proprietary cloud used)  
* signed with your keys (only you can push ROM updates to your phone)  
* bootloader is relocked after first install: no rogue 'fastboot boot/fastboot flash' commands can be issued to your device  
* secure boot aka Android Verified Boot  
* F-Droid and F-Droid Privileged Extension to allow easy installation of FOSS apps  
* Optional: OpenGapps (while still retaining locked bootloader)  
* Optional: Magisk (while still retaining locked bootloader)  
* Optional: misc. patches (custom bootanimation, add/delete entries in recovery menu, add new permission toggles and so)  


## Initial setup  

The ROM will be built on your computer/server so, the prerequisites of AOSP has to be met: [Establishing a Build Environment](https://source.android.com/setup/build/initializing)  
Keep in mind that AOSP and Chromium will be built in the process, so the whole build will take many hours.  
I'm personally building this on a (quite powerful) computer (4c/8t Core i7, 32GB RAM, 1 TB SSD NVMe) with Ubuntu 18.04 and the whole thing is compiled under 5 hours (maybe less, can't remember)  


## Building  

./prerequisites.sh  
./build.sh [-m] DEVICE FORCE_BUILD AOSP_BUILD AOSP_BRANCH

The -m argument will build Magisk in.  

DEVICE --> The codename of the device you would like to build, e.g., sargo for Pixel 3a
FORCE --> Whether or not you want to force a re-build, even when an updated version isn't available. You should probably use false so as to avoid unnecessary rebuilds
AOSP_BUILD --> The android build ID, as listed on https://developers.google.com/android/ota. For example: qq2a.200405.005, for the April 2020 update
AOSP_BRANCH --> The source code tag, as listed on https://android.googlesource.com/platform/manifest/+refs. For example: android-10.0.0_r33, for the qq2a.200405.005 build

Putting it all together:

./build.sh sargo false qq2a.200405.005 android-10.0.0_r33

## Flashing  

Once the build is done you'll find yourself with flashable zip files within $CHAOSP_DIR/out/release-$DEVICE-$BUILD_NUMBER/  
You can now follow the same guide than RattlesnakeOS: [Flashing guide](https://github.com/dan-v/rattlesnakeos-stack/blob/9.0/FLASHING.md)  


## TODO  
* add a -g argument to the script to toggle or not, the integration of OpenGapps (actually, pico package is always built-in)  
* add an option in recovery menu to delete all Magisk-related settings/modules to avoid a lock-out situation (when bootloop occured, etc.)  
* replace Chromium with Bromite as a Browser/WebView  
* add an option to use [microG Project](https://microg.org/) instead of proprietary Google Play Services (when using OpenGapps)  
* find a way to build Magisk during the building of CHAOSP instead of downloading Magisk releases zip files from GitHub  


## Credits  
* @thestinger for his work on the now deceased CopperheadOS, and newly started [GrapheneOS](https://github.com/GrapheneOS)  
* @dan-v for his work on [RattlesnakeOS](https://github.com/dan-v/rattlesnakeos-stack) from which near the integrity of the build Go template is re-used here  
* @topjohnwu for the only FOSS Android rooting solution: [Magisk](https://github.com/topjohnwu/Magisk)  
* @anestisb for his handy script allowing us to retrieve important missing and non-git commited binary blobs for our devices : [android-prepare-vendor](https://github.com/anestisb/android-prepare-vendor)  
* @PabloCastellano for his handy DTB extracter script: [extract-dtb](https://github.com/PabloCastellano/extract-dtb)  
* [OpenGapps](https://github.com/opengapps) for their [aosp_build](https://github.com/opengapps/aosp_build) project  
* many different people for the useful FOSS marketplace: [F-Droid](https://github.com/f-droid)  


