**Building raylib sources on Raspberry Pi:**

_Step 1._ Make sure you have installed in your Raspberry Pi OpenAL Soft library for audio:

    sudo apt-get install libopenal1 libopenal-dev

_Step 2._ Navigate from command line to `raylib/src/` folder and type:

    make PLATFORM=PLATFORM_RPI

**Building raylib examples on Raspberry Pi:**

_Step 1._ Make sure you have installed in your Raspberry Pi the OpenAL Soft library for audio:

    sudo apt-get install libopenal1 libopenal-dev

_Step 2._ Navigate from command line to `raylib/examples/` folder and type:

    make PLATFORM=PLATFORM_RPI
