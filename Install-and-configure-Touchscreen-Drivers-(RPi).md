# Background

tslib is a touchscreen library that handles input from Touchscreen devices. Touchscreen devices vary enormously in the types and variety of data they generate, and tslib attempts to take this raw stream, filter and scale it such that an application can use it without needing to know too much about the specific touchscreen itself.

Linking tslib directly with you own application doesn't really help, as although that would enable you app to read from and react to the Touchscreen device, it wouldn't benefit from RayLib's gesture detection, collision detection etc.,

The solution is to use **ts_uinput** this uses tslib to create a raw input device stream, which can be read by RayLib. The advantage being that this additional device stream has already passed through tslib's modules, so is scaled, and dejittered before RayLib reads it.

## Prerequisites for running tslib and ts_uinput on a Raspberry Pi

To build **tslib** on a Raspberry Pi, you will need automake and libtool:

    sudo apt-get install automake libtool

Cloning the tslib repository doesn't create the default /etc/ts.conf file, so install tslib:

    sudo apt-get install ts-lib

## Clone the tslib repository

Navigate to the root directory beneath which you want to install the tslib source

    git clone git://github.com/kergoth/tslib.git
    cd tslib
    ./autogen.sh
    ./configure
    make
    sudo make install

finally, to make sure the library is available to other applications:

    sudo cp -P /usr/local/lib/libts* /lib/arm-linux-gnueabihf/

## Configuring tslib

tslib works by reading the raw event stream from the touchscreen device, and passing this through a series of modules, each of which aims to address a specific issue - scaling or dejittering for example.

Modules are loaded and configured using the the file ts.conf found in

    /etc/ts.conf

Edit this file and make sure the following settings are present (and uncommented):

    module_raw input
    module variance delta=30
    module dejitter delta=100
    module linear

## Calibrating the touchscreen

tslib calibration data is held in the following file:

    /etc/pointercal

This file doesn't need to be edited manually. Instead, calibration data is captured and stored using a separate program called **ts_calibrate** which guides the user through tapping on 5 separate positions on the touchscreen. However I did find that ts_calibrate would fail with an fopen error if it couldn't find the pointercal file, so first create an empty file:

    sudo touch /etc/pointercal

The **ts_calibrate** procedure can be run as often as you like, and it is particularly important to do so if the on-screen cursor doesn't seem to align accurately with the point you touched.

I found I needed to explicitly specify the location for the various parameters when running both **ts_calibrate** and **ts_uinput**

The important aspects of the following command string are that our Touchscreen appears to the Pi as /dev/fb1 and its raw event stream is available as /dev/input/event3 **Note:** these may vary depending on the particular configuration of your RPi - what devices are attached etc.

    sudo TSLIB_FBDEVICE=/dev/fb1 TSLIB_TSDEVICE=/dev/input/event3 TSLIB_CALIBFILE=/etc/pointercal TSLIB_CONFFILE=/etc/ts.conf TSLIB_PLUGINDIR=/usr/local/lib/ts ts_calibrate

The Calibration procedure is very straightforward, and just involves tapping on the screen as each of the 5 crosses is displayed.

Once calibrated, the touchscreen can be tested using:

    sudo TSLIB_FBDEVICE=/dev/fb1 TSLIB_TSDEVICE=/dev/input/event3 TSLIB_CALIBFILE=/etc/pointercal TSLIB_CONFFILE=/etc/ts.conf TSLIB_PLUGINDIR=/usr/local/lib/ts ts_test

Then, to create the input event stream that is going to be read by RayLib, run the following:

    sudo TSLIB_FBDEVICE=/dev/fb1 TSLIB_TSDEVICE=/dev/input/event3 TSLIB_CALIBFILE=/etc/pointercal TSLIB_CONFFILE=/etc/ts.conf TSLIB_PLUGINDIR=/usr/local/lib/ts ts_uinput -v -d

The arguments at the end are as follows:

-v  Verbose (lists more information as ts_uinput loads)
-d  Run as a daemon

If all is well, ts_uinput should report that it has created a new virtual input device input4, which should be listed as event4 and mouse2 if you enter the following:

    ls /dev/input

**Note:** that if the new Input Event appears as anything other than /dev/input/event4 you will need to edit /raylib/src/core.c and change the #DEFAULT_TOUCH_DEV setting towards the top of the file, and re-build RayLib

To run ts_uinput as a daemon automatically, each time the RPi is re-booted, you will need to place the ts_uinput startup script in /etc/rc.local just above the default section that prints the IP address, and omit 'sudo' from the front as scripts in rc.local run as root.

