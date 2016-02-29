To build your raylib game for Linux you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code and examples. 

raylib has been tested in the following Linux distros: Ubuntu

###Installing dependencies

raylib requires some libraries on Linux, those libraries are GLFW3 (Windows and input management) and OpenAL Soft (audio system management). Just install them:

####Installing GLFW3
This library depends on some other libraries:

    sudo apt-get install mesa-common-dev-dev libx11 libxrandr libXi-dev-dev xorg-dev libgl1-mesa-dev libglu1-mesa-dev

Download and build GLFW3 from git (you also need cmake tool):

    git clone https://github.com/glfw/glfw.git glfw
    cd glfw
    cmake .
    sudo make install

####Installing OpenAL Soft
    sudo apt-get install libopenal1 libopenal-dev

###Compiling raylib source code

Before compiling your game, raylib library must be recompiled for Linux. To do that, just navigate to `raylib\src\` folder and run:

    make PLATFORM=PLATFORM_DESKTOP

NOTE: By default raylib is compiled for OpenGL 3.3 Core graphics API backend; to compile for OpenGL 1.1 graphics API just type:

    make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

###Compiling raylib examples
Just move to folder `raylib/examples/` and run:

    make PLATFORM=PLATFORM_DESKTOP

To compile just one specific example:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP

To force one example recompile:

    make core_basic_window PLATFORM=PLATFORM_DESKTOP -B