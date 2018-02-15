To build your raylib game for GNU/Linux you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code, examples and templates that you can use on your projects.  

This guide is for all GNU/Linux distros (I will use APT as package manager, for Debian based distros).

### Install basics and useful packages
You have to install GCC compilers, Make tool and git (for downloading raylib repository):

    sudo apt install build-essential git

### Compile raylib source code
#### Install raylib dependencies
raylib requires GLFW3 (Windows and input management) and OpenAL Soft (audio system management). Just install them:

    sudo apt install libopenal-dev

GLFW3 depends on some other libraries:

    sudo apt install mesa-common-dev libx11-dev libxrandr-dev libxi-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev libglew-dev

Now, download GLFW3 from sources and build it (you also need cmake tool; if you don't have it, just do: `sudo apt install cmake`):

    wget https://github.com/glfw/glfw/releases/download/3.2.1/glfw-3.2.1.zip
    unzip glfw-3.2.1.zip
    cd glfw-3.2.1
    cmake  -DBUILD_SHARED_LIBS=ON
    sudo make install

#### Build raylib using make
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

Install the library to the standard directories, or remove it:

    sudo make install # Static version.
    sudo make install RAYLIB_LIBTYPE=SHARED # Dynamic shared version.
    
    sudo make uninstall
    sudo make uninstall RAYLIB_LIBTYPE=SHARED

_NOTE:_ raylib is versatile and can be be built on several platforms and supports numerous graphics apis, among other things. Please read raylib/src/Makefile to see the available options and values.  

### Compile raylib examples

If you have installed raylib with `make install`, just move to the folder `raylib/examples` and run:

    make PLATFORM=PLATFORM_DESKTOP

If raylib was installed with `make install RAYLIB_LIBTYPE=SHARED`, notify the examples with:

    make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED

A more detailed command for the OpenGL ES GRAPHICS variant on Ubuntu 16.04 is:

    make  PLATFORM=PLATFORM_DESKTOP  GRAPHICS=GRAPHICS_API_OPENGLES_20 \
        CFLAGS="-fPIC -I/usr/include/GL"  RAYLIB_LIBTYPE=SHARED \
        LDFLAGS='-L/usr/local/lib/raysan5 -lGLESv2  -lglfw3'
    
To compile just one specific example, adding flags as needed:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP

To force recompile one example:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP -B

The games folder can be made the same way as the examples. Have fun!

### Build raylib source code using meson

Building with meson on Linux is easy. If you have all the dependencies installed just use the following:

```
git clone https://github.com/raysan5/raylib.git raylib
cd raylib
meson builddir
cd builddir
ninja
ninja install
```

In case any dependencies are missing, meson will tell you.

**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"