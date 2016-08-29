One of the objectives of raylib was minimizing external dependencies. Developing a full-videogame-featured library avoiding external dependencies is a hard task.

Here it is a list with the external dependencies used by raylib; note that most of those dependencies are [single-header public-domain libraries](https://github.com/nothings/stb) that are [directly copied into raylib repository](https://github.com/raysan5/raylib/tree/develop/src/external).

External dependencies:

Library | Used Version | raylib module | Notes
--- | :---: | :---: | ---
[GLFW3](http://www.glfw.org/) | 3.2 | [core](https://github.com/raysan5/raylib/blob/develop/src/core.c) | Window and input management on desktop platforms
[GLAD](https://github.com/raysan5/raylib/blob/develop/src/external/glad.h) | 0.1.9a3 | [rlgl](https://github.com/raysan5/raylib/blob/develop/src/rlgl.c) | Extensions initialization for OpenGL 3.3 Core (*single-header*)
[stb_image](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image.h) | 2.12 | [textures](https://github.com/raysan5/raylib/blob/develop/src/texture.c) | Multiple image formats loading (*single-header*)
[stb_image_resize](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image_resize.h) | 0.91 | [textures](https://github.com/raysan5/raylib/blob/develop/src/texture.c) | Image resizing functions (*single-header*)
[stb_truetype](https://github.com/raysan5/raylib/blob/develop/src/external/stb_truetype.h) | 1.11 | [text](https://github.com/raysan5/raylib/blob/develop/src/text.c) | TTF font data loading (*single-header*)
[stb_image_write](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image_write.h) | 1.02 | [utils](https://github.com/raysan5/raylib/blob/develop/src/utils.c) | PNG image writting (*single-header*)
[stb_vorbis](https://github.com/raysan5/raylib/blob/develop/src/external/stb_vorbis.h) | 1.09 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | OGG audio data loading (*single-header*)
[jar_mod](https://github.com/raysan5/raylib/blob/develop/src/external/jar_mod.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | MOD audio module loading (*single-header*)
[jar_xm](https://github.com/raysan5/raylib/blob/develop/src/external/jar_xm.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | XM audio module loading (*single-header*)
[OpenAL Soft](http://kcat.strangesoft.net/openal.html) | 1.17.2 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | Audio device management
[pthread Win32](https://www.sourceware.org/pthreads-win32/) | 2.9.1 | [physac](https://github.com/raysan5/raylib/blob/develop/src/physac.h) | POSIX style threads on Windows
[lua](https://www.lua.org/about.html) | 5.3.3 | [rlua](https://github.com/raysan5/raylib/blob/develop/src/rlua.h) | raylib lua binding

Note that [physac](https://github.com/raysan5/raylib/blob/develop/src/physac.h) and [rlua](https://github.com/raysan5/raylib/blob/develop/src/rlua.h) are additional raylib modules, single-header and not included in default raylib compilation; if those modules are not required, `pthread Win32` and `lua` dependencies are neither required.

Note that raylib support multiple platforms and, consequently, not all library dependencies from above are the same for all the platforms. As commented, some of the above libraries included in raylib are single-file header-only libraries (`stb_image`, `stb_image_write`, `stb_image_resize`, `stb_vorbis`, `jar_mod`, `jar_xm`, `glad`), those libraries only depend on the C standard library for the target platform (libc, bionic) and are compiled together with raylib, no need for additional library linkage. But some other libraries require external linkage. Here it is a detailed list:

PLATFORM | external dependencies | Notes
--- | :---: | ---
DESKTOP:Windows | `GLFW3`, `OpenGL`, `OpenAL` | GLFW3 also requires linkage with `libgdi32`
DESKTOP:Linux | `GLFW3`, `OpenGL`, `OpenAL` | Linux also requires linkage with `libm`(math), `pthreads`(POSIX threads), `dl`(dynamic loading). GLFW3 also requires linkage with `XWindows` specific libs: `X11`, `Xrandr`, `Xinerama`, `Xi`, `Xxf86vm` and `Xcursor`
DESKTOP:OSX | `GLFW3`, `OpenGL`, `OpenAL` | GLFW3 also requires linkage with `Cocoa` framework
ANDROID| `EGL`, `OpenGLES2.0`, `OpenAL` | Code must be compiled using `Android NDK` libraries, `android_native_app_glue` module is included in raylib shared library compilation and `OpenAL` android implementation also requires linkage against `OpenSLES` audio library.
RASPBERRY PI | `EGL`, `OpenGLES2.0`, `OpenAL`, `bcm_host` | The only external library required (aside of the default ones that come with the system, like `bcm_host`) is `OpenAL`, graphics run in native mode (no `XWindows` required) and inputs are also natively read (no `XWindows` input events)
HTML5 (Web) | `GLFW3`, `WebGL`, `OpenAL` | Code must be compiled using `emscripten SDK`, all required libraries are included in the package, actually it uses javascript versions of `GLFW3` (incomplete implementation) and `OpenAL` (features limited).
OCULUS RIFT CV1 | `GLFW3`, `OpenGL`, `OpenAL`, `libOVR` | Basically, same as DESKTOP platforms and `libOVR` runtime library (link against `libOVRRT32_1.dll`)

Note that raylib design is [very modular](http://www.raylib.com/img/raylib_architecture.png), some modules could be dropped if not required ([audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c), [shapes](https://github.com/raysan5/raylib/blob/develop/src/shapes.c), [models](https://github.com/raysan5/raylib/blob/develop/src/models.c)...) and consequently the libraries used by those modules. Some modules can also be used as standalone, *independently* of raylib: [rlgl](https://github.com/raysan5/raylib/blob/develop/examples/rlgl_standalone.c), [audio](https://github.com/raysan5/raylib/blob/develop/examples/audio_standalone.c).

As stated, one of the raylib goals is to keep external dependencies to minimum and so I keep working to reduce this list as much as possible.




