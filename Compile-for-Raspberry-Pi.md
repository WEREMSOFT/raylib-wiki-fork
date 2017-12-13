To build your raylib game for Raspberry Pi you need to download raylib git repository (develop branch recommended) and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code and examples.

### Supported Devices and OS

Currently ALL Raspberry Pi devices are supported by raylib (1, 2, 3, Zero and variants). OS supported are Raspbian Jessie (latest update) and [Raspbian Stretch](https://www.raspberrypi.org/downloads/raspbian/).

### Supported OpenGL backends

 - OpenGL ES 2.0 in native mode, not X11 (any RPI version with the default non-GL driver)
 - OpenGL 1.1 on X11 desktop mode, (**only RPI 2 and 3**)
 - OpenGL 2.1 on X11 desktop mode, (**only RPI 2 and 3**)

### Installing dependencies

raylib only requires one additional library dependency to the ones that already comes with Raspbian if used in native mode, this library is OpenAL Soft (audio system management). Just install it:

    sudo apt-get install libopenal1 libopenal-dev

**If using raylib on desktop mode**, another additional dependency is required:

    sudo apt-get install libglfw3

### Compiling raylib source code

Before compiling your game, raylib library must be recompiled for Raspbian. To do that, just navigate to `raylib\src\` directory and run one of the following options depending on your needs:

1. To use OpenGL ES 2.0 in native mode (no X11):
```
make PLATFORM=PLATFORM_RPI
```
2. To use desktop OpenGL 1.1 or 2.1 (X11 window)
```
make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_21
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