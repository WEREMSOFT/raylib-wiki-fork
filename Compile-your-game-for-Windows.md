**Building raylib sources on desktop platforms:**

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/src/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: By default raylib is compiled for OpenGL 3.3 Core backend; to compile for OpenGL 1.1 just type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

**Building raylib examples on Windows platforms:**

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/examples/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: Make sure the following libs (and their headers) are accesible to the compiler (placed on right folders):

    libglfw3.a    - GLFW3 (static version)
    libglew32.a   - GLEW, OpenGL extension loading, only required if using OpenGL 3.3
    libopenal32.a - OpenAL Soft, audio device management