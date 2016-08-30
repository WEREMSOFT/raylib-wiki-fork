raylib relies on OpenGL as its main graphic API. Depending on the platform raylib uses different versions of OpenGL by default but it's possible to configure the desired OpenGL API backend on library compilation; just select the appropriate define when compiling [rlgl]() module.

All OpenGL API calls are contained in rlgl module, this module works as a layer to underlying graphic API. Used OpenGL version can be configured on this modules with defines at compile time.

Here it is a table with the different OpenGL configurations supported by raylib on the different platforms:

PLATFORM | OpenGL version | Notes
--- | :---: | ---
DESKTOP:Windows | `OpenGL 1.1`, `OpenGL 2.1`, `OpenGL 3.3` | Desired version can be selected at compile time
DESKTOP:Linux | `OpenGL 1.1`, `OpenGL 2.1`, `OpenGL 3.3` | Desired version can be selected at compile time
DESKTOP:OSX | `OpenGL 1.1`, `OpenGL 2.1`, `OpenGL 3.3` | Desired version can be selected at compile time
ANDROID| `OpenGL ES 2.0` | Selected automatically if `PLATFORM_ANDROID` is defined
RASPBERRY PI | `OpenGL ES 2.0` | Run natively, no XWindows desktop supported.
HTML5 (Web) | `WebGL 1.0 (OpenGL ES 2.0)` | `OpenGL ES 2.0` transformed to `WebGL 1.0` on compilation (using `emscripten SDK`), no emulation layer, only pure `WebGL 1.0` translation.
OCULUS RIFT CV1 | `OpenGL 2.1`, `OpenGL 3.3` | Basically, same as `DESKTOP` platforms. `OpenGL 1.1` not supported because `GL_EXT_framebuffer_object` is required.

Additional backends (not supported by default): 

PLATFORM | OpenGL version | Notes
--- | :---: | ---
DESKTOP:Windows | `OpenGL ES 2.0` | Support through two possible ways: `WGL_EXT_create_context_es2_profile` extension or [ANGLE]() project
DESKTOP:Linux | `OpenGL ES 2.0` | Support through `GLX_EXT_create_context_es2_profile` extension
ANDROID | `OpenGL ES 3.0` | Only about [55% Android devices](https://developer.android.com/about/dashboards/index.html) support it reight now.
RASPBERRY PI | `OpenGL 1.1`, `OpenGL 2.1` | Supported through Gallium OpenGL driver, only available for RPI 2 and 3
HTML5 (Web) | `WebGL 2.0 (OpenGL ES 3.0)` | `OpenGL ES 3.0` transformed to `WebGL 2.0` on compilation (using `emscripten SDK`).

Please let me know if you require one of those backends to be implemented.

Additionally, I'm planning a new graphics backend: a simple 2D software renderer. I think it could be useful on some embedded platforms... still on design phase.

