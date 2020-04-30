raylib is a C library and inevitably it depends on some implementation of [C standard library (libc)](https://en.wikipedia.org/wiki/C_standard_library). 

While working on raylib 3.0 I took some time to analyze that dependency in detail, no plans to remove it (maybe minimize it a bit) but I think it could be useful to have the knowledge where it is required.

### stdlib.h

| module | libc function   | raylib function   |
| :----: | --------------- | ----------------- |
| core                   | srand()         | InitTimer()      |
| core                   | abs(), rand()   | GetRandomValue() |
| core                   | atexit()        | InitKeyboard()<br>InitTerminal() |
| shapes                 | fabs()          | CheckCollisionCircleRec()<br>GetCollisionRec() |
| utils                  | exit()          | TraceLog()       |

### stdio.h

Note that file loading functionality is currently being centralized to `LoadFileData()`/`SaveFileData()`, most of the file access calls below will disappear soon, replaced by memory buffers parsing (Issue [#1232](https://github.com/raysan5/raylib/issues/1232).

| module | libc function   | raylib function   |
| :----: | --------------- | ----------------- |
| text                   | fopen(), fseek(), fread(), fwrite(), fclose()    | LoadBMFont()  |
| text                   | fgets()                | LoadBMFont()   |
| textures               | fopen(), fseek(), fread(), fwrite(), fclose() | ExportImageAsCode()<br>LoadDDS()<br>LoadPKM()<br>LoadKTX()<br>SaveKTX()<br>LoadPVR()<br>LoadASTC() |
| models                 | fopen(), fseek(), fread(), fwrite(), fclose()  | LoadIQM()<br>ExportMesh()<br>LoadModelAnimations()  |
| audio                  | fopen(), fseek(), fread(), fwrite(), fclose()  | ExportWaveAsCode()<br>LoadWAV()<br>SaveWAV()                                                       |
| utils                  | sprintf()  | TraceLog()    |
| utils                  | fopen(), fseek(), fread(), fwrite(), fclose() | LoadFileData()<br>SaveFileData() |

### string.h

| module | libc function   | raylib function   |
| :----: | --------------- | ----------------- |
| core                   | strlen()   | GetFileNameWithoutExt()<br>GetDirectoryPath()<br>GetPrevDirectoryPath()<br>OpenURL()<br>InitEvdevInput()     |
| core                   | strrchr()  | GetExtension()<br>EventThreadSpawn()  |
| core                   | strcmp()   | IsGamepadName()<br>EmscriptenKeyboardCallback()  |
| text                   | strcmp()   | TextIsEqual()   |
| text                   | strcpy()   | TextAppend()<br>TextReplace()   |
| text                   | strncpy()  | TextToUtf8()<br>TextReplace()   |
| text                   | strcat()   | TextJoin()         |
| text                   | strstr()   | *several funcs.*     |
| textures               | strlen()   | ImageTextEx()      |
| raudio                 | strcmp()   | IsFileExtension()  |

### math.h

| module | libc function   | raylib function   |
| :----: | --------------- | ----------------- |
| models, shapes, camera | sinf(), asinf(), cosf()<br>acosf(), sqrtf(), atan2f()              | *several funcs.*     |
| raymath                | sinf(), cosf(), acosf(), tan()<br>fabs(), sqrtf(), fminf(), fmaxf() | *several funcs.*     |
| core                   | tan()                                                            | BeginMode3D()        |
| rlgl                   | atan2()                                                          | SetVrConfiguration() |
| text                   | sqrtf()                                                          | GenImageFontAtlas()  |

### stdarg.h

| module | libc function   | raylib function   |
| :----: | --------------- | ----------------- |
| text   | va\_list, va\_start(), vsprintf(), va\_end()   | TextFormat() |
| utils  | va\_list, va\_start(), vsprintf(), va\_end()   | TraceLog()   |                                                                                            

Considering the size of raylib, there is not much dependency on *libc*... that's an interesting consideration for embedded devices development where custom *libc* implementations could be used.