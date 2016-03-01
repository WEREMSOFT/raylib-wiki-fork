To build your raylib game for HTML5 you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are the [emscripten SDK](http://kripken.github.io/emscripten-site/).

###Installing emscripten

Download emscripten SDK from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the Portable Emscripten SDK for Windows and decompressing it in `C:\emsdk` folder. After that, go to emsdk intallation folder, run `emcmdprompt.bat` and execute the following commands:

    emsdk update
    emsdk list
    emsdk install sdk-1.34.1-64bit

_NOTE: If a newer preconpiled SDK version (newer than sdk-1.34.1-64bit) is available, install new version. Preconpiled SDK already includes latest version of clang, emscripten, python and node.js, so you don't have to install it separately._

###Compiling raylib source code [Optional]

Before compiling your game, raylib library must be recompiled for HTML5. By default, when you install raylib using the Windows Installer, an already pre-compiled raylib HTML5 version is found in `C:\raylib\raylib\src\libraylib.bc`. Notepad++ uses this version but you can regenerate it just recompiling raylib for web (in case you have modified raylib sources or you updated it with GitHub develop branch sources).

###Preparing your raylib game for web
To compile your game for web, code must be slightly adapted. Basically it implies moving all your Update and Draw code to an external function. The reason for that is the way web games work within a browser; the browser needs to control the game loop and just allow and Update-Draw cycle when the tab is active or browser is not minimized. 

To see how code should be refactored to fit compilation for web, check [core_basic_window_web.c](https://github.com/raysan5/raylib/blob/master/examples/core_basic_window_web.c) example.

###Compiling raylib game
Just open your raylib web-prepared game code and run the Notepad++ script named `raylib_compile_emscripten`. Note that script defines some paths to the required tools, check you are using the correct versions of those tools!

Note also that required resources should be embedded into a .data file using the compiler parameter `--preload-file filename.ext` or `--preload-file folder`.

###Testing raylib game
To test the newly created .html file (and its .js and .data), just create a localhost (you can do it using python) and open the web in the browser (required google chrome).