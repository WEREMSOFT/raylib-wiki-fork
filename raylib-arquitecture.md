![image](https://cloud.githubusercontent.com/assets/5766837/15992278/62292086-30c9-11e6-8761-369c2f1010d9.png)

I tried to make raylib as modular as posible, keeping a small number of specific and self-contained modules, organized by its main functionality (and avoiding hundreds of modules depending on other modules, like other C libraries).

raylib main modules are 7: 
 - `core`: Window / Graphic Context / Inputs management.
 - `rlgl`: Graphic API direct access (OpenGL) and pseudo-OpenGL 1.1 translation layer.
 - `shapes`: Basic 2D shapes drawing functions.
 - `textures`: Textures / Image loading and management.
 - `text`: Font data loading and text drawing.
 - `models`: 3D models loading and drawing.
 - `audio`: Audio device management and sounds / music loading and playing.

Those 7 modules share a common header: `raylib.h`, all user functions are defined in that header, despite the fact they are divided internally in 7 modules. That way, user just needs to include `raylib.h` to get all raylib functionality. Other libraries use multiple headers for every module or also headers that refer to other headers; I don't like that approach, I prefer to keep it simpler.

But there is more. In my design I also tried to keep modularity and independence between modules, so, there are some modules that can be used independently of raylib, as standalone libraries. Those modules are `rlgl` ([example](https://github.com/raysan5/raylib/blob/develop/examples/rlgl_standalone.c)) and `audio` ([example](https://github.com/raysan5/raylib/blob/develop/examples/audio_standalone.c)); since raylib 1.3, new standalone modules were also added to the library with some additional features: `camera`, `gestures` and `raygui`. Those modules can also be used as standalone (like `rlgl` and `audio`), so the available equivalent headers for the user... despite the fact that `camera`and `gestures` are currently included in `raylib.h`.

But there is more. Some modules are defined as header-only files, like the well-known [stb libraries](https://github.com/nothings/stb). Being header-only means that header also contains the functions implementation; that's extremely useful in case you have a library (a bunch of functions) that you only want to drop on your code-base to cover a very specific functionality. Actually, raylib uses internally some [of those libraries](https://github.com/raysan5/raylib/tree/develop/src/external). Similar to stb libraries, raylib defines some modules in the same way: `raymath`, `physac`, `raygui` and `easings`. Right now only `raymath` is used internally by raylib, the other three are provided to the user.
 
And that's currently the raylib internal structure.

Now, working in raylib 1.5, I'm reviewing part of that structure: `raygui`and `physac` have been ported to header-only independent modules and thinking to do the same with `camera` and `gestures`; but porting a module to header-only is not trivial, that module has to minimize external dependencies and global variables and give a very specific functionality; let's say it has to be completely portable.
