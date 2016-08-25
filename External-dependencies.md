One of the main objectives with raylib was minimizing external dependencies. Developing a full-videogame-featured library avoiding external dependencies is a hard task.

Here it is a list with the external dependencies used by raylib; note that most of those dependencies are [single-header public-domain libraries](https://github.com/nothings/stb) that are [directly copied into raylib repository](https://github.com/raysan5/raylib/tree/develop/src/external).

External dependencies:

Library | Used Version | raylib module | Notes
--- | :---: | :---: | ---
[GLFW3](http://www.glfw.org/) | 3.2 | [core](https://github.com/raysan5/raylib/blob/develop/src/core.c) | Window and input management on desktop platforms
[GLAD](https://github.com/raysan5/raylib/blob/develop/src/external/glad.h) | 0.1.9a3 | [rlgl](https://github.com/raysan5/raylib/blob/develop/src/rlgl.c) | Extensions initialization for OpenGL 3.3 Core
[stb_image](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image.h) | 2.12 | [textures](https://github.com/raysan5/raylib/blob/develop/src/texture.c) | Multiple image formats loading
[stb_image_resize](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image_resize.h) | 0.91 | [textures](https://github.com/raysan5/raylib/blob/develop/src/texture.c) | Image resizing (multiple algorythms)
[stb_truetype](https://github.com/raysan5/raylib/blob/develop/src/external/stb_truetype.h) | 1.11 | [text](https://github.com/raysan5/raylib/blob/develop/src/text.c) | TTF font data loading
[stb_image_write](https://github.com/raysan5/raylib/blob/develop/src/external/stb_image_write.h) | 1.02 | [utils](https://github.com/raysan5/raylib/blob/develop/src/utils.c) | PNG image writting
[stb_vorbis](https://github.com/raysan5/raylib/blob/develop/src/external/stb_vorbis.h) | 1.09 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | OGG audio data loading
[jar_mod](https://github.com/raysan5/raylib/blob/develop/src/external/jar_mod.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | MOD audio module loading
[jar_xm](https://github.com/raysan5/raylib/blob/develop/src/external/jar_xm.h) | 0.01 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | XM audio module loading
[OpenAL Soft](http://kcat.strangesoft.net/openal.html) | 1.17.2 | [audio](https://github.com/raysan5/raylib/blob/develop/src/audio.c) | Audio device management (multiple backends)
[pthread Win32](https://www.sourceware.org/pthreads-win32/) | 2.9.1 | [physac](https://github.com/raysan5/raylib/blob/develop/src/physac.h) | POSIX style threads on Windows
[lua](https://www.lua.org/about.html) | 5.3.3 | [rlua](https://github.com/raysan5/raylib/blob/develop/src/rlua.h) | raylib lua binding





