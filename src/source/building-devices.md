<!--
   Copyright 2010 The Android Open Source Project

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->

# Building for devices #

This page complements the main page about [Building](building.html) with
information that is specific to individual devices.

With the current release, it is possible to build for
Nexus 10, for Nexus 7 (Wi-Fi), and for some variants of Galaxy Nexus.
The exact level of functionality for each device depends on the availability
of the relevant proprietary hardware-specific binaries.

All configurations of Nexus 10 can be used. On those devices, graphics, audio,
Wi-Fi, Bluetooth, camera, NFC, GPS and orientation sensors are functional.

The Wi-Fi variants of Nexus 7 can be used. On Nexus 7, graphics and audio are
functional, as well as Wi-Fi and Bluetooth. Due to hardware differences, do
not use 4.1.1 on a Nexus 7 that was originally sold with 4.1.2 or newer.
The Mobile variant is not supported.

The variants of Galaxy Nexus that can be used are the GSM/HSPA+ configuration
"maguro" (only if it was originally sold with a "yakju" or "takju" operating
system) and the VZW CDMA/LTE configuration "toro". On those devices, graphics
and audio are functional, as well as Wi-Fi, Bluetooth, and access to the
respective cellular networks. NFC and the orientation sensors are functional.

The Sprint CDMA/LTE configuration "toroplus" of Galaxy Nexus is supported
experimentally, in the jb-mr1-dev-plus-aosp branch. On that configuration,
the cellular network is not functional,
and the other peripherals work like they do on "toro".

The Motorola Xoom can be used in the Wi-Fi configuration "wingray"
sold in the USA, with Android 4.1.2. Graphics and audio are functional
as well as Wi-Fi and Bluetooth and the orientation sensors.

All configurations of Nexus S and Nexus S 4G can be used with Android 4.1.2.
On those devices all the peripherals are functional: graphics, audio, Wi-Fi,
Bluetooth, cell networks, sensors, camera, hardware codecs, NFC, GPS.

In addition, [PandaBoard](http://pandaboard.org) a.k.a. "panda" can be used
in the jb-mr1-dev-plus-aosp branch, but is considered experimental.
The specific details to use a PandaBoard with the Android Open-Source Project
are in the file `device/ti/panda/README` in the source tree.

Nexus One a.k.a. "passion" is obsolete, was experimental in gingerbread,
and can't be used with newer versions of the Android Open-Source
Project.

Android Developer Phones (ADP1 and ADP2, a.k.a. "dream" and "sapphire") are
obsolete, were experimental in froyo, and can't be used with
newer versions of the Android Open-Source Project.

## Building fastboot and adb ##

If you don't already have those tools, fastboot and adb can be built with
the regular build system. Follow the instructions on the page about
[building](building.html), and replace the main `make` command with

    $ make fastboot adb

## Booting into fastboot mode ##

During a cold boot, the following key combinations can be used to boot into fastboot mode,
which is a mode in the bootloader that can be used to flash the devices:

Device   | Keys
---------|------
manta    | Press and hold both *Volume Up* and *Volume Down*, then press and hold *Power*
grouper  | Press *Power* for a second, and press *Volume Down* when the bootloader logo appears
tilapia  | Press *Power* for a second, and press *Volume Down* when the bootloader logo appears
phantasm | Power the device, cover it with one hand after the LEDs light up and until they turn red
maguro   | Press and hold both *Volume Up* and *Volume Down*, then press and hold *Power*
toro     | Press and hold both *Volume Up* and *Volume Down*, then press and hold *Power*
toroplus | Press and hold both *Volume Up* and *Volume Down*, then press and hold *Power*
panda    | Press and hold *Input*, then press *Power*
wingray  | Press and hold *Volume Down*, then press and hold *Power*
crespo   | Press and hold *Volume Up*, then press and hold *Power*
crespo4g | Press and hold *Volume Up*, then press and hold *Power*

Also, the command `adb reboot bootloader` can be used to reboot from
Android directly into the bootloader with no key combinations.

## Unlocking the bootloader ##

It's only possible to flash a custom system if the bootloader allows it.

The bootloader is locked by default. With the device in fastboot mode, the
bootloader is unlocked with

    $ fastboot oem unlock

The procedure must be confirmed on-screen, and deletes the user data for
privacy reasons. It only needs to be run once.

All data on the phone is erased, i.e. both the applications' private data
and the shared data that is accessible over USB, including photos and
movies. Be sure to make a backup of any precious files you have before
unlocking the bootloader.

On Nexus 10, after unlocking the bootloader, the internal storage is
left unformatted and must be formatted with

    $ fastboot format cache
    $ fastboot format userdata

The bootloader can be locked back with

    $ fastboot oem lock

Note that this erases user data on Xoom (including the shared USB data).

## Obtaining proprietary binaries ##

The Android Open-Source Project can't be used
from pure source code only, and requires additional hardware-related proprietary
libraries to run, specifically for hardware graphics acceleration.

Official binaries for Nexus S, Nexus S 4G, Galaxy Nexus, Nexus 7,
Nexus 10 and PandaBoard
can be downloaded from
[Google's Nexus driver page](https://developers.google.com/android/nexus/drivers),
which add access to additional hardware capabilities with non-Open-Source code.

When using the master branch for a device, the binaries for the most
recent numbered release are the ones that should be used in the master
branch.

### Extracting the proprietary binaries ###

Each set of binaries comes as a self-extracting script in a compressed archive.
After uncompressing each archive, run the included self-extracting script
from the root of the source tree, confirm that you agree to the terms of the
enclosed license agreement, and the binaries and their matching makefiles
will get installed in the `vendor/` hierarchy of the source tree.

### Cleaning up when adding proprietary binaries ###

In order to make sure that the newly installed binaries are properly
taken into account after being extracted, the existing output of any previous
build needs to be deleted with

    $ make clobber

## Picking and building the configuration that matches a device ##

The steps to configure and build the Android Open-Source Project
are described in the page about [Building](building.html).

The recommended builds for the various devices are available through
the lunch menu, accessed when running the `lunch` command with no arguments:

Device   | Branch                       | Build configuration
---------|------------------------------|------------------------
manta    | android-4.2.1_r1.2           | full_manta-userdebug
grouper  | android-4.2.1_r1.2           | full_grouper-userdebug
maguro   | android-4.2.1_r1.2           | full_maguro-userdebug
toro     | android-4.2.1_r1.2           | full_toro-userdebug
toroplus | jb-mr1-dev-plus-aosp         | full_toroplus-userdebug
panda    | jb-mr1-dev-plus-aosp         | full_panda-userdebug
wingray  | android-4.1.2_r1             | full_wingray-userdebug
crespo   | android-4.1.2_r1             | full_crespo-userdebug
crespo4g | android-4.1.2_r1             | full_crespo4g-userdebug

Do not use 4.1.1 on a Nexus 7 that was originally sold with 4.1.2
or newer.

## Flashing a device ##

Set the device in fastboot mode if necessary (see above).

An entire Android system can be flashed in a single command: this writes
the boot, recovery and system partitions together after verifying that the
system being flashed is compatible with the installed bootloader and radio,
and reboots the system. This also erases all the user data, similarly to
`fastboot oem unlock` mentioned earlier.

    $ fastboot -w flashall

Note that filesystems created via fastboot on Motorola Xoom aren't working
optimally, and it is strongly recommended to re-create them through recovery

    $ adb reboot recovery

Once in recovery, open the menu (press Power + Volume Up), wipe the cache
partition, then wipe data.

## Restoring a device to its original factory state ##

Factory images
for Nexus 10,
for Nexus Q,
for Nexus 7 (all variants),
for Galaxy Nexus (GSM/HSPA+ "yakju" and "takju",
and CDMA/LTE "mysid" and "mysidspr"),
and
for Nexus S and Nexus S 4G (all variants)
are available from
[Google's factory image page](https://developers.google.com/android/nexus/images).

Factory images for the Motorola Xoom are distributed directly by Motorola.
