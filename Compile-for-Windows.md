To build your raylib game for Windows, raylib installer comes with all the required tool; it includes a ready to use code editor: Notepad++ with some useful scripts ready and it also includes a compiler with basic C libraries: MinGW.

If you want to use another code editor like Visual Studio or another compiler package you can also use them but this instructions below are focused on raylib standard development tools: Notepad++ and MinGW.

### Installing raylib

Just download raylib installer and run it. It installs Notepad++ and MinGW, custom versions for better integration with raylib.

NOTE: Make sure the following libs (and their headers) are accesible to the compiler (placed on right folders):

    libglfw3.a    - GLFW3 (static version)
    libopenal32.a - OpenAL Soft, audio device management

### Compiling raylib source code

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/src/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: By default raylib is compiled for OpenGL 3.3 Core backend; to compile for OpenGL 1.1 just type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

### Compiling your raylib game

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/examples/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP