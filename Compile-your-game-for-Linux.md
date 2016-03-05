**This section is still in construction; it may not works!**

To build your raylib game for Linux you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code and examples.

This guide is for Ubuntu/Debian distros, that use APT as package manager; it is tested on Ubuntu 14.04.

# Install basis and useful packages

You have to install GCC compilers, Make tool and git (for download raylib repository):

    sudo apt install build-essential git

# Install raylib
## Install raylib dependencies

raylib requires some libraries on Linux, those libraries are GLFW3 (Windows and input management) and OpenAL Soft (audio system management). Just install them:

## Install GLFW3
This library depends on some other libraries:

    sudo apt install mesa-common-dev libx11-dev libxrandr-dev libXi-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev libglew-dev

Now, you need to download GLFW3 from git and build it (you also need cmake tool; if you have not, just do: `sudo apt install cmake`):

    git clone https://github.com/glfw/glfw.git glfw
    cd glfw
    cmake .
    sudo make install

## Installing OpenAL Soft

    sudo apt install libopenal1 libopenal-dev

## Compiling raylib source code

First, you need to download raylib repository from git; after you can compile raylib for GNU/Linux:

    git clone https://github.com/raysan5/raylib.git raylib
    cd raylib/src/
    make PLATFORM=PLATFORM_DESKTOP

_NOTE:_ By default raylib is compiled for OpenGL 3.3 Core graphics API backend; to compile for OpenGL 1.1 graphics API just type:

    make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

If you want, you can move the library and header in standards directories:

    sudo cp libraylib.a /usr/local/lib/libraylib.a
    sudo mkdir /usr/local/include/raylib # If there is not the directory
    sudo cp raylib.h /usr/local/include/raylib/raylib.h

**WARNING:** if you move raylib in standard directories, you must include the library with `#include <raylib/raylib.h>` in your projects. When you compile it on other operating systems, they don't find the raylib header. This is a solution:

    #ifdef __gnu_linux__
        #include <raylib/raylib.h>
    #else
        #include <raylib.h>
    #endif

###Compiling raylib examples **(WIP)**
Just move to folder `raylib/examples/` and run:

    make PLATFORM=PLATFORM_DESKTOP

To compile just one specific example:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP

To force one example recompile:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP -B