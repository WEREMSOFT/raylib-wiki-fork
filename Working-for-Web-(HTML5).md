To build your raylib game for HTML5 you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are the [emscripten SDK](https://emscripten.org/).

### Installing emscripten

Download [emscripten SDK](https://emscripten.org/docs/getting_started/downloads.html) from [GitHub](https://github.com/emscripten-core/emsdk), download as a zip and decompress it in `C:\emsdk` folder. 

`emsdk` requires [**Python**](https://www.python.org/downloads/) and [**Git**](https://git-scm.com/downloads) installed and accesible from system path to be called from `emsdk prompt`.

After decompression and installing required tools (and making sure `python` and `git` can be called from command line), go to `emsdk` installation folder and run `emcmdprompt.bat`. 

Execute the following commands to install and activate latest emscripten tools:

```
emsdk install latest
emsdk activate latest
```

On Linux and macOS you will also need to set the proper environment so that raylib build system can find the Emscripten compiler:

`source ./emsdk_env.sh`

_NOTE: Updated installation notes are always [available here](https://emscripten.org/docs/getting_started/downloads.html)._

### Compiling raylib source code

Before compiling your game, raylib library **must be recompiled for HTML5**, generating `libraylib.bc`.

#### Using Makefile

* For Windows users :

Before compiling raylib, make sure all paths to emscripten (`EMSDK_PATH`) and tools are correctly configured on `C:/raylib/raylib/src/Makefile`, you must verify [this lines](https://github.com/raysan5/raylib/blob/master/src/Makefile#L149).

To compile raylib source code, just execute Notepad++ script: `raylib_makefile` and `SET PLATFORM=PLATFORM_WEB`. That script just calls the following `make` line (in case you're are working on a custom environment):

`make PLATFORM=PLATFORM_WEB -B`

* For Linux users :

You have to modify 3 variables :(`EMSDK_PATH`) (`PYTHON_PATH`) and (`PATH`). Just verify inside the emsdk directory if you have correct paths for (`EMSCRIPTEN_PATH`), (`CLANG_PATH`) and (`NODE_PATH`). (`EMSDK_PATH`) corresponds to path where you downloaded emscripten.

You must set the (`PATH`) to :
```
$(shell printenv PATH):$(EMSDK_PATH):$(EMSCRIPTEN_PATH):$(CLANG_PATH):$(NODE_PATH):$(PYTHON_PATH)
```

As example, your `MakeFile` on Linux should look similar to:
```
    EMSDK_PATH         ?= /path/to/emsdk
    EMSCRIPTEN_PATH    ?= $(EMSDK_PATH)/upstream/emscripten
    CLANG_PATH          = $(EMSDK_PATH)/upstream/bin
    PYTHON_PATH         = /path/to/python
    NODE_PATH           = $(EMSDK_PATH)/node/12.9.1_64bit/bin
    PATH = $(shell printenv PATH):$(EMSDK_PATH):$(EMSCRIPTEN_PATH):$(CLANG_PATH):$(NODE_PATH):$(PYTHON_PATH)
```
After the path configuration, just execute the following command:

`make PLATFORM=PLATFORM_WEB -B`

Generated `libraylib.bc` is placed in `raylib\src\libraylib.bc` directory.

#### Using CMake

If you prefer to use `CMake` instead of the plain `Makefile` provided, just execute the following command:

```
cmake -H. -Bbuild -DPLATFORM=Web -GNinja -DCMAKE_TOOLCHAIN_FILE=cmake/emscripten.cmake
cmake --build build
```

### Preparing your raylib game for web

To compile your game for web there are two possible scenarios:

1. Avoiding raylib `while(!WindowShouldClose())` loop

Main reason to avoid the standard game `while()` loop is related to the way browsers work; the browser needs to control the executed process and just allow a single Update-Draw execution in a time-frame, so execution could be controlled and locked when required (i.e. when the tab is not active or browser is minimized). More details [here](https://emscripten.org/docs/porting/emscripten-runtime-environment.html#browser-main-loop)

To avoid the loop, code must be slightly adapted. Basically it implies moving all your `Update` and `Draw` code to an external function, possibly called `UpdateDrawFrame()`, and consequently manage all required variables from a global context.

For a simple example on code refactoring for web, check [core_basic_window_web.c](https://github.com/raysan5/raylib/blob/master/examples/core/core_basic_window_web.c) example. For more complex examples, just check `raylib/templates` directory, all templates are ready to work on any platform with no changes required.

Avoiding `while()` loop will give better control of the program to the browser and it will run at full speed in the web.

2. Using standard raylib `while(!WindowShouldClose())` loop

There could be some situations where the game `while()` loop could not be avoided and users need it to deal with it. For those situations, emscripten implemented two solutions: [`EMTERPRETER`](https://emscripten.org/docs/porting/emterpreter.html#emterpreter-async-run-synchronous-code) and the new [`ASYNCIFY`](https://emscripten.org/docs/porting/emterpreter.html#comparison-to-asyncify). Both methods basically detect synchronous code and move those pieces of code to an asynchronous mode at compilation.

raylib examples [`Makefile`](https://github.com/raysan5/raylib/blob/master/examples/Makefile) has been adapted to use [`ASYNCIFY`](https://github.com/raysan5/raylib/blob/master/examples/Makefile#L236) and, despite the theorical performace lost, the tested results are great.

### Compiling raylib game

To compile your raylib code for web, it's recommended you use an already setup `Makefile` ready for that, `raylib/templates/simple_game/Makefile` could be used for reference, just copy it to your code folder.

Before compiling the game, review the copied `Makefile` to make sure emscripten sdk path (`EMSDK_PATH`) and other paths are correctly set. Also review the following `Makefile` variables: `PROJECT_NAME`, `RAYLIB_PATH`, `PROJECT_SOURCE_FILES`.

Once `Makefile` has been reviewed, to compile raylib source code, just execute the following `make` call from command line:

    make PLATFORM=PLATFORM_WEB -B

_Note that required resources should be embedded into a `.data` file using the compiler parameter `--preload-file filename.ext` or `--preload-file folder` (already configured in the `Makefile` to use `resources` directory)._

Compilation will generate several output files:

```
  project_name.html  > HTML5 game shell to execute the game
  project_name.js    > Glue code to load WebAssembly program
  project_name.wasm  > WebAssembly program
  project_name.data  > Required resources packaged
```

### Testing raylib game

To test the newly created `.html` file (and its .wasm, .js and .data), just create a `localhost` (you can do it using python) and open the web in the browser.

Create `localhost` using python, make sure you set the local host to the same folder where your `.html` file is located or keep in mind that the directory from which you set the localhost is the base directory for browser access.

Using Python 2.7.x, create a localhost using:

    python -m SimpleHTTPServer 8080

Using Python 3.x, create a localhost using:

    python3 -m http.server 8080

Finally, access your game in the browser using:

    localhost:8080/project_name.html
