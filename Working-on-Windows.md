## Building Library

To build your raylib game for Windows you need to download raylib git repository. raylib already comes with ready-to-use makefiles and CMake system to compile source code, examples and templates. You can alternatively download *raylib Windows Installer*.

**raylib Windows Installer** comes with all the required tools to develop with raylib, those tools are:
 * C Compiler (TCC or MinGW) - To compile the code, it includes all required system libraries.
 * Notepad++ (preconfigured) - To edit code, it includes ready-to-use scripts to compile code and examples.
 * raylib library - Including, source, release, examples and templates.

If you want, you can a different code editor (e.g. Visual Studio) or another compiler, but the following instructions are focused on raylib standard development tools: Notepad++ and TCC/MinGW.

### Build raylib using Notepad++ script

Just open `raylib/src/raylib.h` source file on Notepad++ and execute (F6) the script `raylib_source_compile`

### Build raylib using make

Using MinGW make tool, just navigate from command line to `raylib/src/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

By default raylib is compiled for OpenGL 3.3 Core backend; to compile for OpenGL 1.1 just type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

## Building Examples

### Build example using Notepad++ script

Just open your example source file on Notepad++ and execute (F6) the script `raylib_compile_execute`

### Build ALL examples using make

Using MinGW make tool, just navigate from command line to `raylib/examples/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP