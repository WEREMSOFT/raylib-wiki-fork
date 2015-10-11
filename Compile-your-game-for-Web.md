#Compile for Web

To build your raylib game for HTML5 you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are the [emscripten SDK](http://kripken.github.io/emscripten-site/).

##Installing emscripten

Download emscripten SDK from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the Portable Emscripten SDK for Windows and decompress it in `C:\emsdk` folder. After that, follow the portable version installation instructions.

##Compiling raylib source code [Optional]

Before compiling your game, raylib library must be recompiled for HTML5. By default, when you install raylib using the Windows Installer, an already pre-compiled raylib HTML5 version is found in C:\raylib\raylib\src\libraylib.bc. Notepad++ use this version but you can regenerate it just recompiling raylib for web.

##Preparing your raylib game for web
To compile your game for web, code must be slightly adapted. It means moving all your Update and Draw code to an external function. The reason for that is the way web games work within a browser; basically, the browser needs to control the game loop and just allow and Update-Draw cycle when the tab is selected or browser is not minimized. As reference, you can check `core_basic_window` sample adapted for web [here](https://github.com/raysan5/raylib/blob/master/examples/core_basic_window_web.c).

##Compiling raylib game
Just open your raylib web-prepared game code and run the Notepad++ script named `raylib_compile_emscripten`. Note that script defines some paths to the tools, check you are using the correct versions of those tools!