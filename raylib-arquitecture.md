![image](https://github.com/raysan5/raylib/blob/master/docs/images/raylib_architecture.png)

raylib is a very modular library, defined by a small number of specific and self-contained modules. Every module is organized by its main functionality (avoiding hundreds of modules depending on other modules, like other C libraries).

**raylib main modules are 7:**
 - [`core`](https://github.com/raysan5/raylib/blob/master/src/core.c): Window / Graphic Context / Inputs management.
 - [`rlgl`](https://github.com/raysan5/raylib/blob/master/src/rlgl.c): Graphic API (OpenGL) wrapper and pseudo-OpenGL 1.1 translation layer.
 - [`shapes`](https://github.com/raysan5/raylib/blob/master/src/shapes.c): Basic 2D shapes drawing functions.
 - [`textures`](https://github.com/raysan5/raylib/blob/master/src/textures.c): Textures / Image loading and management.
 - [`text`](https://github.com/raysan5/raylib/blob/master/src/text.c): Font data loading and text drawing.
 - [`models`](https://github.com/raysan5/raylib/blob/master/src/models.c): 3D models loading and drawing.
 - [`audio`](https://github.com/raysan5/raylib/blob/master/src/audio.c): Audio device management and sounds / music loading and playing.

Those 7 modules share a common header: [`raylib.h`](https://github.com/raysan5/raylib/blob/master/src/raylib.h), all user functions are defined in that header, despite the fact they are divided internally in 7 modules. That way, user just needs to include `raylib.h` to get all raylib functionality. Other libraries use one headers for every module (that way user can choose included modules) or also headers that refer to other headers; raylib uses a simpler approach, easier for novice (and expert) users.

A part of those 7 main modules, some other modules have been implemented with additional features:
 - [`raymath`](https://github.com/raysan5/raylib/blob/develop/src/raymath.h): Vector3, Matrix and Quaternion math related functions
 - [`camera`](https://github.com/raysan5/raylib/blob/develop/src/camera.h): 3D Camera system (free, 1st person, 3rd person, custom)
 - [`gestures`](https://github.com/raysan5/raylib/blob/develop/src/gestures.h): Touch gestures detection and processing (Tap, Swipe, Drag, Pinch)
 - [`raygui`](https://github.com/raysan5/raygui): Simple IMGUI system with several controls for tools development
 - [`easings`](https://github.com/raysan5/raylib/blob/develop/src/easings.h): Easing functions for animations (based on [Robert Penner](http://robertpenner.com/easing/) implementation)
 - [`physac`](https://github.com/victorfisac/Physac): 2D physics library (collision detection, resolution, dinamics...)

Most of the modules have been designed to be as decoupled as possible from other modules, so, some modules can be used independently of raylib, as standalone libraries. Highlight two of those modules: `rlgl` ([example](https://github.com/raysan5/raylib/blob/develop/examples/others/rlgl_standalone.c)) and `audio` ([example](https://github.com/raysan5/raylib/blob/develop/examples/others/audio_standalone.c)).

Most of the secondary modules can also be used as standalone libraries: `raymath`, `camera`, `gestures`, `raygui`, `easings`, `physac`. All those modules are distributed as configurable single-file header-only libraries to be independently added to any project. Being header-only means that header also contains the functions implementation; that's extremely useful in case you have a library (a bunch of functions) that you only want to drop on your code-base to cover a very specific functionality. But creating a header-only module is not trivial, that module has to minimize external dependencies and global variables and give a very specific functionality; let's say it has to be completely portable.

*NOTE: `raymath`, `camera` and `gestures` are compiled by default with raylib.*

raylib also uses some external libraries, most of them included as single-file header-only, like the well-known [stb libraries](https://github.com/nothings/stb) or [similar ones](https://github.com/raysan5/raylib/tree/develop/src/external).

And that's currently the raylib internal structure.