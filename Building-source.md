

**Building raylib sources on Raspberry Pi:**

_Step 1._ Make sure you have installed in your Raspberry Pi OpenAL Soft library for audio:

    sudo apt-get install libopenal1 libopenal-dev

_Step 2._ Navigate from command line to `raylib/src/` folder and type:

    make PLATFORM=PLATFORM_RPI

**Building raylib sources for Web (HTML5)**

_Step 1._ Make sure you have installed emscripten SDK:

> Download latest version from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the [Portable Emscripten SDK for Windows](https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-1.35.0-portable-64bit.zip) and decompress it in `C:\emsdk` folder. After that, follow the portable version installation instructions.

_Step 2._ Open `raylib/src/makefile` on Notepad++ and run the script named `raylib_makefile_emscripten`