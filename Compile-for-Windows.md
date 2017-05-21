To build your raylib game for Windows, raylib installer comes with all the required tool; it includes a ready to use code editor: Notepad++ with some useful scripts ready and it also includes MinGW compiler with basic C libraries.

If you want to use another code editor like Visual Studio or another compiler package you can also use them but this instructions below are focused on raylib standard development tools: Notepad++ and MinGW.

### Installing raylib

Just download raylib installer and run it. It installs Notepad++ and MinGW, custom versions for better integration with raylib.

Add the bin path of raylib installer's GNU tools to your PATH environment variable.
_e.g. C:\raylib\MinGW\bin_

NOTE: Make sure the following libs (and their headers) are accessible to the compiler (This should be taken care of after following the above steps):

    GLFW3 - libglfw3.a (static version)
    OpenAL Soft - libOpenAL32.a (static version)

### Compiling raylib source code

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/src/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: By default raylib is compiled for OpenGL 3.3 Core backend; to compile for OpenGL 1.1 just type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

### Compiling your raylib game

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/examples/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP