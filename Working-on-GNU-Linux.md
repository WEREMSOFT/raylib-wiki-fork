## Building Library

To build your raylib game for GNU/Linux you need to download raylib git repository. raylib already comes with ready-to-use makefiles and CMake system to compile source code, examples and templates.  

This guide is for all GNU/Linux distros, just note that APT is used as package manager for Debian based distros.

#### Install required tools
You need a **GCC** (or alternative C99 compiler), **make** and **git** (to download raylib repo). 

    sudo apt install build-essential git

Optionally, you could use **CMake** building system.

    sudo apt install cmake

#### Install required libraries
You need to install some required libraries; **ALSA** for audio, **Mesa** for OpenGL accelerated graphics and **X11** for windowing system.

    sudo apt install libasound2-dev mesa-common-dev libx11-dev libxrandr-dev libxi-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev

### Build raylib using make
You can compile three different types of raylib library:

* The static library (default method)
* The dynamic shared library (often used on Linux)
* The web library

Download the raylib repository from [Github](https://github.com/raysan5/raylib), then compile it with:

    git clone https://github.com/raysan5/raylib.git raylib
    cd raylib/src/
    make PLATFORM=PLATFORM_DESKTOP # To make the static version.
    make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED # To make the dynamic shared version. - Recommended
    make PLATFORM=PLATFORM_WEB # To make web version.

**Warning:** if you want to compile a different type of library (static, ...), you must type `make clean` before the compiling.

Install the library to the standard directories, `usr/local/lib` and `/usr/local/include`, or remove it:

    sudo make install # Static version.
    sudo make install RAYLIB_LIBTYPE=SHARED # Dynamic shared version.
    
    sudo make uninstall
    sudo make uninstall RAYLIB_LIBTYPE=SHARED

_NOTE:_ raylib is configurable and can be be built in a variety of ways. Please read [`src/Makefile`](https://github.com/raysan5/raylib/blob/master/src/Makefile), [`src/config.h`](https://github.com/raysan5/raylib/blob/master/src/config.h) and [`raylib.h`](https://github.com/raysan5/raylib/blob/master/src/raylib.h) itself for a full listing of available options and values.

### Build raylib using CMake

Building with cmake on Linux is easy, just use the following:
```
git clone https://github.com/raysan5/raylib.git raylib
cd raylib
mkdir build && cd build
cmake -DSHARED=ON -DSTATIC=ON ..
make
make install
```
In case any dependencies are missing, cmake will tell you.

_NOTE:_ raylib is configurable and can be be built in a variety of ways. See the [CMake Build Options](https://github.com/raysan5/raylib/wiki/CMake-Build-Options) for a full listing of available options and values. Alternatively, you may use the curses UI provided by `ccmake(1)` for interactively configuring raylib.

## Building Examples

If you have installed raylib with `make install`, Just move to the folder `raylib/examples` and run:

    make PLATFORM=PLATFORM_DESKTOP

If raylib was installed with `make install RAYLIB_LIBTYPE=SHARED`

    make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED

A more detailed command for the OpenGL ES GRAPHICS variant on Ubuntu 16.04 is:

    make  PLATFORM=PLATFORM_DESKTOP  GRAPHICS=GRAPHICS_API_OPENGLES_20 RAYLIB_LIBTYPE=SHARED USE_EXTERNAL_GLFW=TRUE \
        CFLAGS="-fPIC -I/usr/include/GL" LDFLAGS='-L/usr/local/lib/raysan5 -lGLESv2 -lglfw3'
    
To compile just one specific example, add flags as needed:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP

To force recompile one example:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP -B

The `raylib/games` folder can be made the same way as the examples. Have fun!


### Link raylib with system GLFW

Instead of using the embedded GLFW, you can download GLFW3 and build it from source (it requires CMake).

    wget https://github.com/glfw/glfw/releases/download/3.2.1/glfw-3.2.1.zip
    unzip glfw-3.2.1.zip
    cd glfw-3.2.1
    cmake -DBUILD_SHARED_LIBS=ON
    sudo make install

Now with GLFW installed, you may build raylib while specifying `USE_EXTERNAL_GLFW`:

```
make USE_EXTERNAL_GLFW=TRUE
# or
cmake -DUSE_EXTERNAL_GLFW=ON
```

## Install on GNU Linux

If you are a packager for a Distribution not listed here, please take the time and create a raylib package.

### openSUSE
openSUSE has raylib in the official repository. Install it via:

```
zypper in raylib-devel
```

Users of older openSUSE versions will have to add the [devel:libraries:c_c++ devel project](https://build.opensuse.org/project/show/devel:libraries:c_c++).

Add the repo, refresh sources and install raylib:
```
zypper ar -f http://download.opensuse.org/repositories/devel:/libraries:/c_c++/openSUSE_Factory/devel:libraries:c_c++.repo
zypper ref
zypper in raylib-devel
```

**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:ray@raylib.com "Ramon Santamaria - Ray San"

### "Unity build" - the simplest way to build a project.

On linux you can easily build your project (here titled linux_platform.c) without having to mess with make files.  It's fast, and easier to reason about. This is called a unity build (no it has nothing to do with the Unity game engine)

Simple create a build file, and give it executable permissions;
```
touch build.sh
chmod 700 build.sh
```

The edit build.sh to read:
```
set -e    # if error, stop the build
cc -g -std=c99 -c -I /opt/raylib/src linux_platform.c -o ./obj/linux_platform.o
cc -o linux_platform  obj/linux_platform.o -s -Wall -std=c99 -I/opt/raylib/src -L/opt/raylib/release/libs/linux -lraylib -lGL -lm -lpthread -ldl -lrt -lX11
./linux_platform
```