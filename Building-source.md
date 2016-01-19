**Building raylib sources on desktop platforms:**

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/src/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: By default raylib is compiled for OpenGL 3.3 Core backend; to compile for OpenGL 1.1 just type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP GRAPHICS=GRAPHICS_API_OPENGL_11

**Building raylib sources on Raspberry Pi:**

_Step 1._ Make sure you have installed in your Raspberry Pi OpenAL Soft library for audio:

    sudo apt-get install libopenal1 libopenal-dev

_Step 2._ Navigate from command line to `raylib/src/` folder and type:

    make PLATFORM=PLATFORM_RPI

**Building raylib sources for Android:**

_Step 1._ Make sure you have installed Java Runtime 8 (JRE), Android SDK, Android NDK and Apache Ant tools:

> Download and Install [Java SE Runtime Environment 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)

> Download and decompress on C: [Android SDK r24](http://dl.google.com/android/android-sdk_r24.4.1-windows.zip)

> Download and decompress on C: [Android NDK r10e](http://dl.google.com/android/ndk/android-ndk-r10e-windows-x86.exe)

> Download and decompress on C: [Apache Ant 1.9.6](http://apache.rediris.es//ant/binaries/apache-ant-1.9.6-bin.zip)

_Step 2._ Create the following environment variables with the correct paths: 

    ANDROID_SDK_TOOLS = C:\android-sdk\platform-tools
    ANDROID_NDK_ROOT = C:\android-ndk-r10e
    ANT_HOME = C:\apache-ant-1.9.6

_Step 3._ Make sure latest Android Platform Tools and desired SDKs are installed, use SDK Manager tool:

    C:\android-sdk\SDK Manager.exe

_Step 4._ Navigate from command line to folder `raylib/template_android/` and type:

    %ANDROID_NDK_ROOT%\ndk-build

NOTE: libraylib.a will be generated in folder `raylib/src_android/obj/local/armeabi/`, it must be copied
to Android project; if using `raylib/template_android` project, copy it to `raylib/template_android/jni/libs/`.

**Building raylib sources for Web (HTML5)**

_Step 1._ Make sure you have installed emscripten SDK:

> Download latest version from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the [Portable Emscripten SDK for Windows](https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-1.35.0-portable-64bit.zip) and decompress it in `C:\emsdk` folder. After that, follow the portable version installation instructions.

_Step 2._ Open `raylib/src/makefile` on Notepad++ and run the script named `raylib_makefile_emscripten`