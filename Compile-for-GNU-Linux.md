To build your raylib game for GNU/Linux you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code, examples and templates that you can use on your projects.

This guide is for all GNU/Linux distros (I will use APT as package manager, for Debian based distros).

### Install basis and useful packages
You have to install GCC compilers, Make tool and git (for downloading raylib repository).

    sudo apt install build-essential git

### Compile raylib source code
#### Install raylib dependencies
raylib requires GLFW3 (Windows and input management) and OpenAL Soft (audio system management). Just install them:

    sudo apt install libopenal-dev

GLFW3 depends on some other libraries:

    sudo apt install mesa-common-dev libx11-dev libxrandr-dev libxi-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev libglew-dev

Now, you need to download GLFW3 from sources and build it (you also need cmake tool; if you don't have, just do: `sudo apt install cmake`):

    wget https://github.com/glfw/glfw/releases/download/3.2.1/glfw-3.2.1.zip
    unzip glfw-3.2.1.zip
    cd glfw
    cmake .
    sudo make install

#### Build raylib source code using make
You can compile three different type of raylib library:

* The static library (default method);
* The dynamic shared library;
* The web library.

First of all, you need to download raylib repository from Github; after you can compile it:

    git clone https://github.com/raysan5/raylib.git raylib
    cd raylib/src/
    make PLATFORM=PLATFORM_DESKTOP # To make the static version.
    make PLATFORM=PLATFORM_DESKTOP RAYLIB_LIBTYPE=SHARED # To make the dynamic shared version.
    make PLATFORM=PLATFORM_WEB # To make web version.

**Warning:** if you want to compile a different type of library (static, ...), you must type `make clean` before the compiling.

_NOTE:_ By default raylib is compiled for OpenGL 3.3 Core graphics API backend; to compile for OpenGL 1.1 graphics API just add `GRAPHICS=GRAPHICS_API_OPENGL_11` to the make command.

If you want, you can install the library in the standard directories, or remove it:

    sudo make install # Static version.
    sudo make install RAYLIB_LIBTYPE=SHARED # Dynamic shared version.
    
    sudo make uninstall
    sudo make uninstall RAYLIB_LIBTYPE=SHARED

### Compile raylib examples
Just move to folder `raylib/examples/` and run:

    make PLATFORM=PLATFORM_DESKTOP
    make RAYLIB_LIBTYPE=SHARED # Link to dynamic libraylib.so.  

To compile just one specific example:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP

To force one example recompile:

    make core/core_basic_window PLATFORM=PLATFORM_DESKTOP -B

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