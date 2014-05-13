# ANT in Android
This repository provides instructions on how to integrate ANT support into an Android platform build, and is targeted towards device manufacturers looking to add ANT support into their Android devices.

## How to Add ANT to Android
Before you begin, talk to your chipset vendor to ensure your chip is ANT capable, and to obtain ANT enabled firmware as well as any other chip-specific customizations you may require.

### Obtain Source
1. Clone this repository locally.
2. Copy the appropriate manifest, into the ```.repo/local_manifests/``` directory of your Android Platform source, creating the local_manifests directory if needed. The following manifests are available:
    * **ANT_manifest.xml** - This is the recommended package for most platforms as it pulls in the latest stable components of the ANT Android System Layer. 
    * **Android_System_Package_2-3-0** - This package is ideal for adding ANT to Android 4.1.2 using chipsets that do not have a dedicated ANT channel (see System under Components, below). This package is available under legacy/android-4.1.2_r1/.
3. Execute ```repo sync``` in a shell. This will pull in the following components into your platform:
    * ANT HAL
    * ANT HAL Service
    * ANT Radio Service
    * ANT+ Plugins Service

 See "Components" section below for more details.
4. Verify that ANT was synced by checking if the directory ```external/ant-wireless/``` exists.

### Configure
1. Configure ANT to build:

    i. Add the following to ```device/<company>/<board>/BoardConfig.mk```: 

      ```Makefile
      BOARD_ANT_WIRELESS_DEVICE := "<Chipset>"
      ```
     Where ```<Chipset>``` is the chipset code provided to you by your chip vendor. Note the double quotes in the above line, they are important to include.

    ii. Add the following to ```device/<company>/<board>/device.mk```:

     ```Makefile
     $(call inherit-product, external/ant-wireless/build/ant-wireless.mk)
     ```
2. Add ANT as an airplane mode radio:
    1. Copy ```ToggleableANTRadio.patch``` from the ANT In Android repository, from under ```android_patches/airplane_mode``` into ```frameworks/base/```.
    2. In ```frameworks/base/```:

     ```Shell
     git am ToggleableANTRadio.patch
     ```
3. Apply any other customizations required for proper operation on your device such as:
    * Proprietary firmware scripts
    * Integration with Bluetooth stack

### Test
1. Build the system following your platform's build steps.
2. Verify that ANT functions:
    1. Check that ANT built. After a build has been performed, the following should  be present in the resulting image:
        * ```/system/lib/libantradio.so```
        * ```/system/app/AntHalService.apk```
        * ```/system/app/ANTRadioService.apk```
        * ```/system/app/AntPlusPlugins.apk```
        * ```/system/xbin/antradio_app``` (on debug builds)
    2. Test ANT functionality.
    
        1. Test basic ANT communication by executing ```antradio_app```.
        2. Validate ANT functionality by running through the ANT Validation Procedure. Contact Dynastream Innovations for the latest ANT Verification Package.

## Components
This section describes the components that make up an Android build with ANT support.

### Platform
These components are the underlay of the ANT Software stack, and are necessary for ANT support. ANT Wireless does not provide these components.

* **Chipset** - ANT support can be added to devices using one of many ANT enabled multi-mode communication chipsets. Ask your chip vendor about which chips support ANT.
* **Firmware** - Proprietary firmware scripts (or custom kernel modules for certain chips) are required for all supported chipsets. Ask your chip vendor for ANT enabled firmware for your chipset.
* **Android OS** - ANT support is available for Android Platform Version 2.1 and greater.

### System
ANT Wireless provides an open source implementation of this layer which is suitable for most devices. 
Some chipsets require a custom implementation of this layer as they do not provide a dedicated ANT channel.
For such chipsets, integration with the Bluetooth stack is required to communicate with and control the chip.
Talk to your chipset provider for more details.

* **ANT HAL** - This C library communicates with the firmware to send and receive messages as well as control the chipset.
* **ANT HAL Service** - This Android application is a JNI wrapper for ANT HAL.

### Applications
These are applications that will be pre-installed on your device to allow Android Applications that use ANT and ANT+ technology to operate using an easy to use API. These applications are also available through the PlayStore and are not required to be system applications.

* **ANT Radio Service** - This Android service allows apps to use a simplified interface to communicate using the ANT protocol.
* **ANT+ Plugins** - This Android service allows apps to use a simplified interface to communicate using ANT+ Profiles.

# Legal
***
Copyright 2014 Dynastream Innovations
***
