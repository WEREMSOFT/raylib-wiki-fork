**Building raylib examples on Windows platforms:**

_Step 1:_ Using MinGW make tool, just navigate from command line to `raylib/examples/` folder and type:

    mingw32-make PLATFORM=PLATFORM_DESKTOP

NOTE: Make sure the following libs (and their headers) are accesible to the compiler (placed on right folders):

    libglfw3.a    - GLFW3 (static version)
    libglew32.a   - GLEW, OpenGL extension loading, only required if using OpenGL 3.3
    libopenal32.a - OpenAL Soft, audio device management

**Building raylib examples on Raspberry Pi:**

_Step 1._ Make sure you have installed in your Raspberry Pi the OpenAL Soft library for audio:

    sudo apt-get install openal1

_Step 2._ Navigate from command line to `raylib/examples/` folder and type:

    make PLATFORM=PLATFORM_RPI

**Building raylib examples for HTML5 (emscripten):**

_Step 1._ Make sure you have installed emscripten SDK:

> Download latest version from [here](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). I recommend downloading the [Portable Emscripten SDK for Windows](https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-1.35.0-portable-64bit.zip) and decompress it in `C:\emsdk` folder. After that, follow the portable version installation instructions.

_Step 2._ Open `raylib/examples/makefile` on Notepad++ and run the script named `raylib_makefile_emscripten`

NOTE: At this moment, raylib examples are not ready to be directly compile for HTML5, code needs to be reorganized due to the way web browsers work. To see how code should be refactored to fit compilation for web, check [core_basic_window_web.c](https://github.com/raysan5/raylib/blob/master/examples/core_basic_window_web.c) example.

**Building raylib project for Android (using template):**

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

_Step 4._ To compile project, navigate from command line to folder `raylib/template_android/` and type:

    %ANDROID_NDK_ROOT%\ndk-build

_Step 5._ To generate APK, navigate to folder `raylib/template_android/` and type:

    %ANT_HOME%\bin\ant debug

_Step 6:_ To install APK into connected device (previously intalled drivers and activated USB debug mode on device):

    %ANT_HOME%\bin\ant installd

_Step 7:_ To view log output from device:

    %ANDROID_SDK_TOOLS%\adb logcat -c
    %ANDROID_SDK_TOOLS%\adb -d  logcat raylib:V *:S

**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"