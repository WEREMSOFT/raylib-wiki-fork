raylib 2.0 has no **external** dependencies, all required libraries are included with raylib sources.

Internally, raylib uses some libraries developed by other people, most of them used for some very specific task, like loading a specific fileformat or initialize audio device.

Here is the list of dependencies used by raylib. Note that most of these dependencies are [single-header public-domain libraries](https://github.com/nothings/stb) that are [included in raylib repository](https://github.com/raysan5/raylib/tree/master/src/external).

Dependencies:

Library | Used Version | raylib module | Notes
--- | :---: | :---: | ---
[GLFW3](http://www.glfw.org/) | 3.3-dev | [rglfw](https://github.com/raysan5/raylib/blob/master/src/rglfw.c) | Window and input management on desktop platforms
[GLAD](https://github.com/raysan5/raylib/blob/master/src/external/glad.h) | 0.1.10a0 | [rlgl](https://github.com/raysan5/raylib/blob/master/src/rlgl.c) | Extensions initialization for OpenGL 3.3 Core (*single-header*)
[stb_image](https://github.com/raysan5/raylib/blob/master/src/external/stb_image.h) | 2.15 | [textures](https://github.com/raysan5/raylib/blob/master/src/texture.c) | Multiple image formats loading (*single-header*)
[stb_image_resize](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_resize.h) | 0.94 | [textures](https://github.com/raysan5/raylib/blob/master/src/texture.c) | Image resizing functions (*single-header*)
[stb_truetype](https://github.com/raysan5/raylib/blob/master/src/external/stb_truetype.h) | 1.15 | [text](https://github.com/raysan5/raylib/blob/master/src/text.c) | TTF font data loading (*single-header*)
[stb_image_write](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_write.h) | 1.05 | [utils](https://github.com/raysan5/raylib/blob/master/src/utils.c) | PNG image writting (*single-header*)
[stb_vorbis](https://github.com/raysan5/raylib/blob/master/src/external/stb_vorbis.h) | 1.10 | [audio](https://github.com/raysan5/raylib/blob/master/src/audio.c) | OGG audio data loading (*single-header*)
[jar_mod](https://github.com/raysan5/raylib/blob/master/src/external/jar_mod.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/master/src/audio.c) | MOD audio module loading (*single-header*)
[jar_xm](https://github.com/raysan5/raylib/blob/master/src/external/jar_xm.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/master/src/audio.c) | XM audio module loading (*single-header*)
[mini_al](https://github.com/dr-soft/mini_al) | 1.17.2 | [audio](https://github.com/raysan5/raylib/blob/master/src/audio.c) | Audio device management

Note that raylib supports multiple platforms and, consequently, not all library dependencies from above are the same for all supported platforms. As commented, some of the above libraries included in raylib are single-file header-only libraries (`stb_image`, `stb_image_write`, `stb_image_resize`, `stb_vorbis`, `jar_mod`, `jar_xm`, `glad`, `mini_al`), most of those libraries only depend on the C standard library for the target platform (msvcrt, libc, bionic) and are compiled together with raylib—no need for additional library linkage.

Despite raylib has, technically speaking, no external dependencies, it requires some platform-specific system libraries to be linked on examples/games compilation. Here it is a table with required libraries:

PLATFORM | external dependencies | Notes
--- | :---: | ---
DESKTOP:Windows | `OpenGL`, `GDI32` | GDI32 is required for window creation
DESKTOP:Linux | `OpenGL`, `X11` | Linux also requires linkage with `libm`(math), `pthreads`(POSIX threads), `dl`(dynamic loading) and `X11` window system specific libs: `X11`, `Xrandr`, `Xinerama`, `Xi`, `Xxf86vm` and `Xcursor`
DESKTOP:OSX | `OpenGL`, `Cocoa` | `Cocoa` framework is required for window creation
ANDROID| `EGL`, `OpenGLES2.0`, `OpenSLES` | Code must be compiled using `Android NDK` libraries
RASPBERRY PI | `EGL`, `OpenGLES2.0`, `bcm_host` | Graphics run in native mode using `bcm_host` (no `XWindows` required) and inputs are also natively read (no `XWindows` input events)
HTML5 (Web) | `WebGL` | Code must be compiled using `emscripten SDK`

Note that the raylib design is [very modular](http://www.raylib.com/images/raylib_architecture.png). Some modules can be dropped if they're not required ([audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c), [shapes](https://github.com/raysan5/raylib/blob/develop/src/shapes.c), [models](https://github.com/raysan5/raylib/blob/develop/src/models.c)...) and consequently the libraries used by those modules. Some modules can also be used as standalone, *independently* of raylib: [rlgl](https://github.com/raysan5/raylib/blob/develop/examples/others/rlgl_standalone.c), [audio](https://github.com/raysan5/raylib/blob/develop/examples/others/audio_standalone.c).

As stated, one of raylib goals is avoid any external dependencies, so I'll keep working to reduce this list as much as possible.