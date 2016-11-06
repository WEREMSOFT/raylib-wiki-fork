Some users want to use raylib with Visual Studio IDE. raylib 1.6 includes [Visual Studio 2015 project templates](https://github.com/raysan5/raylib/tree/develop/project/vs2015) for the library and some examples but maybe you want to configure the library for another Visual Studio version.

Those are the configuration parameters required:

### Configure raylib library project

1. Create a new library project (C)
2. Include the following directories:
    - `$(raylibSrcDir)\external\openal_soft\include`
    - `$(raylibSrcDir)\external\glfw3\include`
    - `$(raylibSrcDir)\external`
3. Preprocesor definitions (for Windows platform):
    - `GRAPHICS_API_OPENGL_33`
    - `PLATFORM_DESKTOP`
4. Advanced Configuration: `Compile as C Code (/TC)`

### Configure raylib game project

1. Create a new Console project
2. Include directory: `$(raylibSrcDir)`
3. Advanced Configuration: `Compile as C Code (/TC)`
4. Linker directories:
    - $(raylibSrcDir)external\glfw3\lib\win32
    - $(raylibSrcDir)external\openal_soft\lib\win32
5. Linker additional dependencies:
    - glfw3.lib
    - openal32.lib



