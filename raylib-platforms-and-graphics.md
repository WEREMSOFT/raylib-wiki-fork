raylib relies on OpenGL as its main graphic API. Depending on the platform, raylib uses different versions of OpenGL by default, but it's possible to configure the desired OpenGL API backend on library compilation; just select the appropriate `#define` when compiling the [rlgl](https://github.com/raysan5/raylib/blob/develop/src/rlgl.c) module.

All OpenGL API calls are contained in the rlgl module. This module works as a layer to the underlying graphic API. The selected OpenGL version can be configured in this module using `#define`s at compile time.

Here is a table with the different OpenGL configurations supported by raylib on the different platforms:

PLATFORM Flag | Platform/OS | OpenGL version | Notes
--- | :-------: | :-------: | ---
PLATFORM_DESKTOP | Windows | `OpenGL 1.1`<br> `OpenGL 2.1`<br> `OpenGL 3.3` | Multiple Windows desktop versions supported: XP, 7, 8, 10
PLATFORM_DESKTOP | Linux | `OpenGL 1.1`<br> `OpenGL 2.1`<br> `OpenGL 3.3` | Multiple distros supported: Ubuntu, Debian, openSUSE, Arch...
PLATFORM_DESKTOP | macOS | `OpenGL 1.1`<br> `OpenGL 2.1`<br> `OpenGL 3.3` | Any version that supports at least OpenGL 1.1
PLATFORM_ANDROID | Android | `OpenGL ES 2.0` | Supported from API level 16 (JellyBean 4.1.x)
PLATFORM_UWP | Windows UWP | `OpenGL ES 2.0` | Supports any UWP device (Desktop App, Phones, Xbox One...). Runs over Direct3D 11 using [ANGLE](https://github.com/Microsoft/angle) emulation layer.
PLATFORM_RPI | Raspbian (native) | `OpenGL ES 2.0` | Runs graphics natively on console, no X11 windowing/inputs system required.
PLATFORM_RPI | Raspbian (X11 desktop) | `OpenGL 1.1`<br> `OpenGL 2.1`<br> `OpenGL 3.3` | Runs on X11 desktop, requires VC4 OpenGL driver enabled.
PLATFORM_WEB | HTML5 (browser) | `WebGL 1.0 (OpenGL ES 2.0)` | `OpenGL ES 2.0` transformed to `WebGL 1.0` on compilation <br>(using `emscripten SDK`), no emulation layer, only pure `WebGL 1.0` translation.

Additional graphic backends (not supported by default): 

PLATFORM Flag | Platform/OS | OpenGL version | Notes
--- | :-------: | :-------: | ---
PLATFORM_DESKTOP | Windows | `OpenGL ES 2.0` | Support through two possible ways: <br> - `WGL_EXT_create_context_es2_profile` extension<br> - [ANGLE](https://github.com/google/angle) project
PLATFORM_DESKTOP | Linux | `OpenGL ES 2.0` | Support through: <br> - `GLX_EXT_create_context_es2_profile` extension
PLATFORM_ANDROID | Android | `OpenGL ES 3.0` | Only about [63% Android devices](https://developer.android.com/about/dashboards/index.html) support it.
PLATFORM_WEB | HTML5 (browser) | `WebGL 2.0` <br>`(OpenGL ES 3.0)` | `OpenGL ES 3.0` transformed to `WebGL 2.0` <br> on compilation (using `emscripten SDK`).

Please let me know if you require one of those backends to be implemented.

Additionally, I'm planning a new graphics backend: a simple 2D software renderer. I think it could be useful on some embedded platforms... still on design phase.
