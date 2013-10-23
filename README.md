ANT in Android
===============

This repository contains an example local_manifest.xml to be placed under .repo/ in your build root directory.  The ANT repositories will then be picked up with a 'repo sync'.

Android Build Setup
===============

***
Copyright 2010 Dynastream Innovations

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
     http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and limitations under the License.
***

#1. OVERVIEW

This is the Android 4.2 release for ANT support.

As of the 4.2 build, chips using Vendor Specific Bluetooth HCI messaging for ANT packets (wl12xx and bcm433x) are not currently supported by this release. However, chip vendors may provide build support for their own products if you inquire with them.   
Implementations using the BTIPS stack or external ANT devices will require a different package.    

#2. CHIP DETAILS
    ===============================================   
    CHIP NAME             | TRANSPORT TYPE            
    ----------------------+------------------------   
    vfs-prerelease        | Character Device / TTY 
    cg29xx                | Character Device / TTY    
    wl12xx                | Vendor Specific BT HCI    
    bcm433x               | Vendor Specific BT HCI    
    ===============================================   

Note: wl12xx and bcm433x require proprietary firmware scripts with ANT support added.

#3. COMPONENTS

## NEW REPOSITORIES
You will require the following ant-wireless repositories:
* ant-wireless/Android_build
* ant-wireless/Android_ANTRadioService
* ant-wireless/Linux_ant-hal
* ant-wireless/Android_ANTHALService
* ant-wireless/Android_antradio-library

#4. CONFIGURATION

## Include the ant-wireless repositories in Android platform source

ant-wireless.mk from Android_build assumes that the path to Android_antradio-library is:

    external/ant-wireless/antradio-library

## Specify ANT Configuration:
 The board configuration files are located in device/\<company\>/\<board\>/

    (e.g. device/ti/blaze/)  

###   Add to BoardConfig.mk:
 a) Define that the board has ANT support:

    BOARD_ANT_WIRELESS_DEVICE := "<chip>"

 where <chip> is one of the CHIP NAME entries in the table above (see 2. CHIP DETAILS).  

###   Add to device.mk:
 a) ANT PRODUCT_PACKAGE definition:  

    (call inherit-product, <path to Android_build project>/ant-wireless.mk)

 where \<path to Android_build project\> points to the location of the ant-wireless/Android_build repository in your build tree.  

#5. BUILD OUTPUT
## All builds
* /system/lib/libantradio.so  
* /system/app/AntHalService.apk  
* /system/app/ANTRadioService.apk  

## Debug builds
For debug version of the Android platform (this include userdebug and eng, but
not user):  
* /system/xbin/antradio_app  
