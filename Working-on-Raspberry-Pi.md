To build your raylib game for Raspberry Pi you just need to download raylib git repository (or wget the current release from [here](https://github.com/raysan5/raylib/releases)). All required libraries come with the raylib, **no additional dependencies are required**. raylib also comes with ready-to-use makefiles to compile source code and examples.


### Supported Devices and OS

Official OS supported is [Raspbian Buster](https://www.raspberrypi.org/downloads/raspbian/), but Stretch currently works too. Following Raspberry Pi devices work flawlessly by raylib:

 - Raspberry Pi Zero (all models)
 - Raspberry Pi 1 (all models)
 - Raspberry Pi 2 (all models)
 - Raspberry Pi 3 (all models)
 - _Raspberry Pi 4 (PLATFORM_DESKTOP only!)_

_NOTE: Currently **Raspberry Pi 4** native mode compilation is not supported, for further information just [check this issue](https://github.com/raysan5/raylib/issues/1041)._



### Supported OpenGL backends

 - OpenGL ES 2.0 in **native mode** (no X11 required)
 - OpenGL 1.1 on X11 desktop mode
 - OpenGL 2.1 on X11 desktop mode

By default, raylib is prepared to be used in native mode, it means, right from the command line, not depending on any windowing system (no X11 required), accessing Broadcom Video driver directly... but some users could prefer to use raylib from a classic X11-based Linux desktop, that's also possible, thanks to the `OpenGL desktop drivers`.

OpenGL desktop mode requires the GL desktop driver and supposedly it only works for RPI 2 and higher, that limit is set for performance reasons... but, if required, GL desktop driver can be also enabled on RPI B/B+ or Zero, [just follow this guide](https://www.raspberrypi.org/forums/viewtopic.php?f=66&t=166495) to enable it.

### Compiling raylib source code

Before you can use raylib in your project you will have to compile it, but this is quick and easy! 

Just navigate to `raylib\src\` directory and run one of the following options depending on your needs:

1. To use OpenGL ES 2.0 in native mode (no X11):
```
make PLATFORM=PLATFORM_RPI
```
2. To use desktop OpenGL 1.1 or 2.1 (X11 window)
```
make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_21
```
NOTE: To use raylib on Raspbian desktop, you need to had previously installed all desktop windowin system libraries, if you just downloaded [Raspbian Desktop](https://www.raspberrypi.org/downloads/raspberry-pi-desktop/), it comes with all required libraries installed but if you're using a plain (no desktop) Raspbian system, you need to install the following components first:
```
sudo apt-get install --no-install-recommends raspberrypi-ui-mods lxterminal gvfs
sudo apt-get install libx11-dev
sudo apt-get install libxcursor-dev
sudo apt-get install libxinerama-dev
sudo apt-get install libxrandr-dev
sudo apt-get install libxi-dev
sudo apt-get install libasound2-dev
sudo apt-get install mesa-common-dev
sudo apt-get install libgl1-mesa-dev
```
raylib also works on [DietPi](https://dietpi.com/) distribution, following libraries are required:
```
sudo apt-get libraspberrypi-dev raspberrypi-kernel-headers
sudo apt-get install build-essential
```

### Compiling raylib examples

Just move to folder `raylib/examples/` and run **ONE of THOSE TWO OPTIONS** (depending on target OpenGL version):

1. To use OpenGL ES 2.0 in native mode (no X11):
```
make PLATFORM=PLATFORM_RPI
```
2. To use desktop OpenGL 1.1 or 2.1 (X11 window)
```
make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_21
```

To compile just one specific example:

1. To use OpenGL ES 2.0 in native mode (no X11):
```
make core/core_basic_window PLATFORM=PLATFORM_RPI
```
2. To use desktop OpenGL 1.1 or 2.1 (X11 window)
```
make core/core_basic_window PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_21
```

Easy.