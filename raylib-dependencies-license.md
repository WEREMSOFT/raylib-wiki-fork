raylib is licensed under an unmodified [zlib/libpng license](https://github.com/raysan5/raylib/blob/master/LICENSE), which is an OSI-certified, BSD-like license that allows static linking with closed source software.

raylib uses several libraries internally, here it is a detailed description of every library license:

Library| License | Notes
----| ---------| -------------
[GLFW](https://github.com/raysan5/raylib/tree/master/src/external/glfw) | [zlib/libpng](https://github.com/raysan5/raylib/blob/master/src/external/glfw/LICENSE.md) | Only `PLATFORM_DESKTOP`, [Official GitHub](https://github.com/glfw/glfw)
[GLAD](https://github.com/raysan5/raylib/blob/master/src/external/glad.h) | [WTFPL/CC0/public domain](https://github.com/Dav1dde/glad#whats-the-license-of-glad-generated-code-101) | Only `PLATFORM_DESKTOP`, [Official GitHub](https://github.com/Dav1dde/glad)
[dirent](https://github.com/raysan5/raylib/blob/master/src/external/dirent.h) | [custom open-source](https://github.com/raysan5/raylib/blob/master/src/external/dirent.h) | Only `Windows` and `MSVC` compiler
[android_native_app_glue](https://github.com/raysan5/raylib/tree/master/src/external/android/native_app_glue) | [Apache v2.0](https://github.com/raysan5/raylib/blob/master/src/external/android/native_app_glue/NOTICE) | Only `PLATFORM_ANDROID`, provided by [Android NDK](https://android.googlesource.com/platform/ndk.git/+/refs/tags/ndk-r21/sources/android/native_app_glue/)
[ANGLE](https://github.com/raysan5/raylib/blob/master/src/external/ANGLE) | [BSD](https://github.com/microsoft/angle/blob/ms-master/LICENSE) | Only `PLATFORM_UWP`, [Official GitHub](https://github.com/microsoft/angle)
[stb_image](https://github.com/raysan5/raylib/blob/master/src/external/stb_image.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_image.h) | [Official GitHub](https://github.com/nothings/stb)
[stb_image_write](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_write.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_write.h) | [Official GitHub](https://github.com/nothings/stb)
[stb_image_resize](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_resize.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_image_resize.h) | [Official GitHub](https://github.com/nothings/stb)
[stb_truetype](https://github.com/raysan5/raylib/blob/master/src/external/stb_truetype.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_truetype.h) | [Official GitHub](https://github.com/nothings/stb)
[stb_vorbis](https://github.com/raysan5/raylib/blob/master/src/external/stb_vorbis.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_vorbis.h) | Using [stb_vorbis fork](https://github.com/BareRose/stb/blob/master/stb_vorbis.h)
[stb_rect_pack](https://github.com/raysan5/raylib/blob/master/src/external/stb_rect_pack.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_rect_pack.h) | [Official GitHub](https://github.com/nothings/stb)
[stb_perlin](https://github.com/raysan5/raylib/blob/master/src/external/stb_perlin.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/stb_perlin.h) | [Official GitHub](https://github.com/nothings/stb)
[rgif](https://github.com/raysan5/raylib/blob/master/src/external/rgif.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/rgif.h) | Based on [gif-h](https://github.com/charlietangora/gif-h)
[miniaudio](https://github.com/raysan5/raylib/blob/master/src/external/miniaudio.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/miniaudio.h) | [Official GitHub](https://github.com/dr-soft/miniaudio)
[dr_flac](https://github.com/raysan5/raylib/blob/master/src/external/dr_flac.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/dr_flac.h) | [Official GitHub](https://github.com/mackron/dr_libs)
[dr_mp3](https://github.com/raysan5/raylib/blob/master/src/external/dr_mp3.h) | [MIT/public domain](https://github.com/raysan5/raylib/blob/master/src/external/dr_mp3.h) | [Official GitHub](https://github.com/mackron/dr_libs)
[jar_mod](https://github.com/raysan5/raylib/blob/master/src/external/jar_mod.h) | [public domain](https://github.com/raysan5/raylib/blob/master/src/external/jar_mod.h) | Based on [HxCModPlayer](https://github.com/jfdelnero/HxCModPlayer)
[jar_xm](https://github.com/raysan5/raylib/blob/master/src/external/jar_xm.h) | [public domain](https://github.com/raysan5/raylib/blob/master/src/external/jar_xm.h) | Based on [libxm](https://github.com/Artefact2/libxm)
[par_shapes](https://github.com/raysan5/raylib/blob/master/src/external/par_shapes.h) | [MIT](https://github.com/raysan5/raylib/blob/master/src/external/par_shapes.h) | [Official GitHub](https://github.com/prideout/par/blob/master/par_shapes.h)
[cglft](https://github.com/raysan5/raylib/blob/master/src/external/cgltf.h) | [MIT](https://github.com/raysan5/raylib/blob/master/src/external/cgltf.h) | [Official GitHub](https://github.com/jkuhlmann/cgltf)
[tinyobj_loader_c](https://github.com/raysan5/raylib/blob/master/src/external/tinyobj_loader_c.h) | [MIT](https://github.com/raysan5/raylib/blob/master/src/external/tinyobj_loader_c.h) | [Official GitHub](https://github.com/syoyo/tinyobjloader-c)

Some notes to consider:

 - Most of those libraries are used to support **some specific fileformat** data loading.
 - Some libraries are only used on **some specific platform** or some specific compiler.

*Please, if you find any license problem with any library that does not allow you to use raylib on a commercial project, jut let me know.*