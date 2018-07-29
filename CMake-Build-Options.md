raylib is configurable and can be be built in a variety of ways. Following is a listing of available CMake options and values, extracted from [`src/CMakeOptions.txt`](https://github.com/raysan5/raylib/blob/a1ec0a5bc33ab8726e55fa433ffc08fe3b42e539/src/CMakeOptions.txt).
> **TIP**:  You can use the curses UI provided by `ccmake(1)` for interactively configuring raylib.

| **Option**                        | **Description**                                           | Possible Values (first is default)|
|:-----------------------------:|:-----------------------------------------------------:|:------------:|
|`USE_EXTERNAL_GLFW`            | Link raylib against system GLFW instead of embedded one | **`OFF`** `ON` `IF_POSSIBLE`|
|`PLATFORM`                     | Platform to build for. | **`Desktop`** `Web` `Android` `Raspberry Pi`|
|`OPENGL_VERSION`               | Force a specific OpenGL Version? | **`OFF`** `3.3` `2.1` `1.1` `ES 2.0`|
 
| **Binary Option**                        | **Description**                                           | **Default** Value|
|:-----------------------------:|:-----------------------------------------------------:|:------------:|
|`USE_WAYLAND`                  | Use Wayland for window creation                     | `OFF`|
|`WITH_PIC`                     | Compile static library as position-independent code | `OFF`|
|`SHARED`                       | Build raylib as a dynamic library | `OFF`|
|`STATIC`                       | Build raylib as a static library | `ON`|
|`MACOS_FATLIB`                 | Build fat library for both i386 and x86_64 on macOS | `ON`|
|`USE_AUDIO`                    | Build raylib with audio module | `ON`|
|`SUPPORT_BUSY_WAIT_LOOP`       | Use busy wait loop for timing sync instead of a high-resolution timer | `ON`|
|`SUPPORT_CAMERA_SYSTEM`        | Provide camera module (camera.h) with multiple predefined cameras: free, 1st/3rd person, orbital | `ON`|
|`SUPPORT_DEFAULT_FONT`         | Default font is loaded on window initialization to be available for the user to render simple text. If enabled, uses external module functions to load default raylib font (module: text) | `ON`|
|`SUPPORT_SCREEN_CAPTURE`       | Allow automatic screen capture of current screen pressing F12, defined in KeyCallback() | `ON`|
|`SUPPORT_GIF_RECORDING`        | Allow automatic gif recording of current screen pressing CTRL+F12, defined in KeyCallback() | `ON`|
|`SUPPORT_GESTURES_SYSTEM`      | Gestures module is included (gestures.h) to support gestures detection: tap, hold, swipe, drag | `ON`|
|`SUPPORT_MOUSE_GESTURES`       | Mouse gestures are directly mapped like touches and processed by gestures system | `ON`|
|`SUPPORT_VR_SIMULATOR`         | Support VR simulation functionality (stereo rendering) | `ON`|
|`SUPPORT_DISTORTION_SHADER`    | Include stereo rendering distortion shader (shader_distortion.h) | `ON`|
|`SUPPORT_FONT_TEXTURE`         | Draw rectangle shapes using font texture white character instead of default white texture. Allows drawing rectangles and text with a single draw call, very useful for GUI systems! | `ON`|
|`SUPPORT_QUADS_DRAW_MODE`      | Use QUADS instead of TRIANGLES for drawing when possible. Some lines-based shapes could still use lines | `ON`|
|`SUPPORT_IMAGE_GENERATION`     | Support proedural image generation functionality (gradient, spot, perlin-noise, cellular) | `ON`|
|`SUPPORT_FILEFORMAT_PNG`       | Support loading PNG as textures | `ON`|
|`SUPPORT_FILEFORMAT_DDS`       | Support loading DDS as textures | `ON`|
|`SUPPORT_FILEFORMAT_HDR`       | Support loading HDR as textures | `ON`|
|`SUPPORT_FILEFORMAT_KTX`       | Support loading KTX as textures | `ON`|
|`SUPPORT_FILEFORMAT_ASTC`      | Support loading ASTC as  textures | `ON`|
|`SUPPORT_FILEFORMAT_BMP`       | Support loading BMP as textures | `OFF`|
|`SUPPORT_FILEFORMAT_TGA`       | Support loading TGA as textures | `OFF`|
|`SUPPORT_FILEFORMAT_JPG`       | Support loading JPG as textures | `OFF`|
|`SUPPORT_FILEFORMAT_GIF`       | Support loading GIF as textures | `OFF`|
|`SUPPORT_FILEFORMAT_PSD`       | Support loading PSD as textures | `OFF`|
|`SUPPORT_FILEFORMAT_PKM`       | Support loading PKM as textures | `OFF`|
|`SUPPORT_FILEFORMAT_PVR`       | Support loading PVR as textures | `OFF`|
|`SUPPORT_FILEFORMAT_OBJ`       | Support loading OBJ file format | `ON`|
|`SUPPORT_FILEFORMAT_MTL`       | Support loading MTL file format | `ON`|
|`SUPPORT_MESH_GENERATION`      | Support procedural mesh generation functions, uses external par_shapes.h library. NOTE: Some generated meshes DO NOT include generated texture coordinates | `ON`|
|`SUPPORT_FILEFORMAT_WAV`       | Support loading WAV for sound | `ON`|
|`SUPPORT_FILEFORMAT_OGG`       | Support loading OGG for sound | `ON`|
|`SUPPORT_FILEFORMAT_XM`        | Support loading XM for sound | `ON`|
|`SUPPORT_FILEFORMAT_MOD`       | Support loading MOD for sound | `ON`|
|`SUPPORT_FILEFORMAT_FLAC`      | Support loading FLAC for sound | `OFF`|
|`SUPPORT_SAVE_PNG`             | Support saving image data in PNG file format | `ON`|
|`SUPPORT_SAVE_BMP`             | Support saving image data in BMP file format | `OFF`|
|`SUPPORT_TRACELOG`             | Show TraceLog() output messages. NOTE: By default LOG_DEBUG traces not shown | `ON`|
|`SUPPORT_FILEFORMAT_FNT`       | Support loading fonts in FNT format | `ON`|
|`SUPPORT_FILEFORMAT_TTF`       | Support loading font in TTF format | `ON`|
|`SUPPORT_IMAGE_MANIPULATION`   | Support multiple image editing functions to scale, adjust colors, flip, draw on images, crop... If not defined only three image editing functions supported: ImageFormat(), ImageAlphaMask(), ImageToPOT() | `ON`|
