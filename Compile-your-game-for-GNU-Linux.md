To build your raylib game for GNU/Linux you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code, examples and templates that you can use on your projects.

This guide is for Ubuntu/Debian distros, that use APT as package manager; it is tested on Ubuntu 14.04.

# Install basis and useful packages
You have to install GCC compilers, Make tool and git (for downloading raylib repository):

    sudo apt install build-essential git

# Compile raylib
## Install raylib dependencies
raylib requires GLFW3 (Windows and input management) and OpenAL Soft (audio system management). Just install them:

    sudo apt install libopenal1 libopenal-dev

GLFW3 depends on some other libraries:

    sudo apt install mesa-common-dev libx11-dev libxrandr-dev libXi-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev libglew-dev

Now, you need to download GLFW3 from sources and build it (you also need cmake tool; if you have not, just do: `sudo apt install cmake`):

    git clone https://github.com/glfw/glfw.git glfw
    cd glfw
    cmake .
    sudo make install

## Compile raylib source code
First, you need to download raylib repository from git; after you can compile it for GNU/Linux:

    git clone https://github.com/raysan5/raylib.git raylib
    cd raylib/src/
    make PLATFORM=PLATFORM_DESKTOP

_NOTE:_ By default raylib is compiled for OpenGL 3.3 Core graphics API backend; to compile for OpenGL 1.1 graphics API just type:

    make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

If you want, you can move the library and header in standards directories:

    sudo cp libraylib.a /usr/local/lib/libraylib.a
    sudo mkdir /usr/local/include/raylib # If there is not the directory
    sudo cp raylib.h /usr/local/include/raylib/raylib.h

# Compile raylib examples
Just move to folder `raylib/examples/` and run:

    make PLATFORM=PLATFORM_DESKTOP

To compile just one specific example:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP

To force one example recompile:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP -B