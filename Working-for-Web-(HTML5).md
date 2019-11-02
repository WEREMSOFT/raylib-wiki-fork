To build your raylib game for HTML5 you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are the [emscripten SDK](http://kripken.github.io/emscripten-site/).

### Installing emscripten

Download [emscripten SDK](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html) from [GitHub](https://github.com/emscripten-core/emsdk), download as a zip and decompress it in `C:\emsdk` folder. It requires Python installed to run the provided scripts, it's recommended [Python 2.7.12](https://www.python.org/downloads/release/python-2716/) or higher.

After that, go to emsdk intallation folder, run `emcmdprompt.bat` and execute the following commands:

```
emsdk install latest-fastcomp
emsdk activate latest-fastcomp
```

This will update the latest version of the **fastcomp** compiler which needed to be able to use the [Emterpreter](https://github.com/emscripten-core/emscripten/wiki/Emterpreter). `EMPTERPRETIFY` is not supported on the WASM backend that is the default one in newer versions.

On Linux and macOS you will also need to set the proper environment so that raylib build system can find the Emscripten compiler:

`source ./emsdk_env.sh`

_NOTE: Updated installation notes are always [available here](https://emscripten.org/docs/getting_started/downloads.html)._

### Compiling raylib source code

Before compiling your game, raylib library must be recompiled for HTML5, generating `libraylib.bc`.

#### Using Makefile
Before compiling raylib, make sure all paths to emscripten sdk path (`EMSDK_PATH`) and version (`EMSCRIPTEN_VERSION`) are correctly configured on `C:/raylib/raylib/src/Makefile`, you must verify [this lines](https://github.com/raysan5/raylib/blob/master/src/Makefile#L149).

To compile raylib source code, just execute Notepad++ script: `raylib_makefile` and `SET PLATFORM=PLATFORM_WEB`. That script just calls the following `make` line (in case you're are working on a custom environment):

`make PLATFORM=PLATFORM_WEB -B`

Generated `libraylib.bc` is placed in `raylib\src\libraylib.bc` directory.

#### Using CMake

```
cmake -H. -Bbuild -DPLATFORM=Web -GNinja -DCMAKE_TOOLCHAIN_FILE=cmake/emscripten.cmake
cmake --build build
```

### Preparing your raylib game for web

To compile your game for web, code must be slightly adapted. Basically it implies moving all your Update and Draw code to an external function. The reason for that is the way web games work within a browser; the browser needs to control the game loop and just allow and Update-Draw in a time-frame and lock the execution when the tab is not active or browser is minimized. More details [here](https://kripken.github.io/emscripten-site/docs/porting/emscripten-runtime-environment.html#browser-main-loop)

To see how code should be refactored for web, check [core_basic_window_web.c](https://github.com/raysan5/raylib/blob/master/examples/core/core_basic_window_web.c) example. For more complex examples, just check `raylib/templates` directory, all templates are ready to work on any platform with no change required.

*NOTE: One mechanism to avoid that code refactoring is using Emterpreter ([more info here](https://kripken.github.io/emscripten-site/docs/porting/emterpreter.html#emterpreter-async-run-synchronous-code)), but there is an important hit on performance.*

### Compiling raylib game

To compile your raylib web-prepared game code for web, it's recommended you use an already setup Makefile ready for that, `raylib/templates/simple_game/Makefile` could be used, just copy it to your code folder.

Before compiling the game, review the copied `Makefile` to make sure emscripten sdk path (`EMSDK_PATH`) and version (`EMSCRIPTEN_VERSION`) are correctly set. Also review the following `Makefile` variables: `PROJECT_NAME`, `RAYLIB_PATH`, `PROJECT_SOURCE_FILES`.

Once `Makefile` has been reviewed, to compile raylib source code, just execute Notepad++ script: `raylib_makefile` and `SET PLATFORM=PLATFORM_WEB`. That script just calls the following `make` line (in case you're are working on a custom environment):

    make PLATFORM=PLATFORM_WEB -B

Note also that required resources should be embedded into a .data file using the compiler parameter `--preload-file filename.ext` or `--preload-file folder` (already configured in the `Makefile`.

Compilation will generate several files, usually:
```
  project_name.html  > HTML5 game shell to execute the game
  project_name.js    > Base code to load WebAssembly program
  project_name.wasm  > WebAssembly program
  project_name.data  > Required resources package
```
### Testing raylib game

To test the newly created `.html` file (and its .js and .data), just create a localhost (you can do it using python) and open the web in the browser (required google chrome).

Create localhost using python, make sure you set the local host from the same folder where your .html file is placed or keep in mind that the directory from which you set the localhost is the base directory for browser access:

    python -m SimpleHTTPServer 8080
