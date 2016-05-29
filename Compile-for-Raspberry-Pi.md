To build your raylib game for Raspberry Pi you need to download raylib git repository and install some dependencies. raylib already comes with ready-to-use makefiles to compile source code and examples. 

###Installing dependencies

raylib only requires one additional library dependency to the ones that already comes with Raspbian, this library is OpenAL Soft (audio system management). Just install it:

    sudo apt-get install libopenal1 libopenal-dev

###Compiling raylib source code

Before compiling your game, raylib library must be recompiled for Raspbian. To do that, just navigate to `raylib\src\` directory and run:

    make PLATFORM=PLATFORM_RPI

###Compiling raylib examples
Just move to folder `raylib/examples/` and run:

    make PLATFORM=PLATFORM_RPI

To compile just one specific example:

    make core_basic_window PLATFORM=PLATFORM_RPI

To force one example recompile:

    make core_basic_window PLATFORM=PLATFORM_RPI-B
