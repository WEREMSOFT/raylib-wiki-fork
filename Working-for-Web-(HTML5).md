To build your raylib game for HTML5 you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are the [emscripten SDK](http://kripken.github.io/emscripten-site/).

### Installing emscripten

Download emscripten SDK from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the Portable Emscripten SDK for Windows and decompressing it in `C:\emsdk` folder. After that, go to emsdk intallation folder, run `emcmdprompt.bat` and execute the following commands:

    emsdk install git-1.9.4
    emsdk update
    emsdk list
    emsdk install sdk-1.38.31-64bit
    emsdk activate latest

_NOTE: If a newer precompiled SDK version (newer than sdk-1.38.31-64bit) is available, install new version. Precompiled SDK already includes version of **clang**, **emscripten**, **python**, **java JDK** and **node.js**, so you don't have to install it separately._

### Compiling raylib source code

Before compiling your game, raylib library must be recompiled for HTML5. By default, when you install raylib using the *Windows Installer*, an already pre-compiled raylib HTML5 version is found in `C:\raylib\raylib\release\libs\html5\libraylib.bc`. Notepad++ uses this version but you can regenerate it just recompiling raylib for web (in case you have modified raylib sources or you updated it wit latesth GitHub sources).

To compile raylib source code, just execute Notepad++ script: `raylib_makefile` and `SET PLATFORM=PLATFORM_WEB`. That script just calls the following `make` line (in case you're are working on a custom environment):

    make PLATFORM=PLATFORM_WEB -B

Generated `libraylib.bc` is placed in `raylib\src\libraylib.bc` directory.

### Preparing your raylib game for web

To compile your game for web, code must be slightly adapted. Basically it implies moving all your Update and Draw code to an external function. The reason for that is the way web games work within a browser; the browser needs to control the game loop and just allow and Update-Draw in a time-frame and lock the execution when the tab is not active or browser is minimized. More details [here](https://kripken.github.io/emscripten-site/docs/porting/emscripten-runtime-environment.html#browser-main-loop)

To see how code should be refactored to fit compilation for web, check [core_basic_window_web.c](https://github.com/raysan5/raylib/blob/master/examples/core/core_basic_window_web.c) example.

*NOTE: We are looking for some mechanism to avoid that code refactoring! [Here](https://kripken.github.io/emscripten-site/docs/porting/emterpreter.html#emterpreter-async-run-synchronous-code) it is a possible solution, despite a hit on performance.*

### Compiling raylib game

Just open your raylib web-prepared game code and run the Notepad++ script named `raylib_makefile`, set `PLATFORM=PLATFORM_WEB`.

Note that script defines some paths to the required tools, check you are using the correct versions of those tools!

Note also that required resources should be embedded into a .data file using the compiler parameter `--preload-file filename.ext` or `--preload-file folder`.

### Testing raylib game
To test the newly created `.html` file (and its .js and .data), just create a localhost (you can do it using python) and open the web in the browser (required google chrome).

Create localhost using python, make sure you set the local host from the same folder where your .html file is placed or keep in mind that the directory from which you set the localhost is the base directory for browser access:

    python -m SimpleHTTPServer 8080
