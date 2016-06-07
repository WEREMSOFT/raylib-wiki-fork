Like any other library or project, a raylib project requires different [dependencies](https://github.com/raysan5/raylib/wiki/External-dependencies) placed accordingly for a correct compilation. It means external library headers and, eventually, some static library.

Dependencies depend on platform, so every platform requires some libraries installed in an specific folder to be accesible for the compiler when compiling raylib or a raylib example/game.

Specific dependencies installation is listed for every platform on this [Wiki](https://github.com/raysan5/raylib/wiki). Actually, there are not many external dependencies for raylib, only a couple (GLFW3, OpenAL Soft) and only in some platforms.

## Dependencies on Windows

For Windows platform, raylib is distributed with a self-contained installer that includes, not only raylib but also a raylib-ready-configured free text editor ([Notepad++](https://notepad-plus-plus.org/)) and a C/C++ compiler with tools ([MinGW](http://www.mingw.org/)).

Notepad++ comes with a bunch of pre-created scripts ready for compiling a raylib project and raylib source for different platforms. Those scripts assume that raylib is installed in the portable folder path: `C:\raylib`

In some cases, it could happen that user decided to move that folder to somewhere else; in that case, raylib paths need to be reconfigured, just updating the `RAYLIB_PATH` variable **at the beginning of every script**.

As a recommendation, if you move `C:\raylib` folder to somewhere else, try to avoid spaces and special characters on the new path, it could generate errors on compilation.

For command-line compilation and custom pipeline configuration, just check [Compile for Windows](https://github.com/raysan5/raylib/wiki/Compile-for-Windows) wiki.


 