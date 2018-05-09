To build your raylib game for Android you need a set of tools for Android. Those tools are basically the Android SDK and Android NDK that can be installed from Android Studio's built in SDK Manager.

### Installing Android tools

_Step 1._ Install Java Runtime 8 (JRE), Android Studio (Android SDK, Android NDK) and Apache Ant tools:

> Download and Install [Java SE Runtime Environment 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)

> Download and Install on C: [Android Studio](https://dl.google.com/dl/android/studio/install/3.0.1.0/android-studio-ide-171.4443003-windows.exe).
Use Android Studio's SDK Manager to install the latest Android Platform Tools and desired SDKs and NDK.

> Download and decompress on C: [Apache Ant](http://www-eu.apache.org/dist//ant/binaries/apache-ant-1.10.1-bin.zip)

_Step 2._ Create the following environment variables:

Android Studio installs the NDK in a ndk-bundle folder inside the SDK folder location (where ever you installed it).

    ANDROID_HOME = C:\android-sdk
    ANDROID_SDK_TOOLS = C:\android-sdk\platform-tools
    ANDROID_NDK = C:\android-sdk\ndk-bundle
    ANT_HOME = C:\apache-ant

### Compiling raylib source code

To compile raylib src it's required to install a [custom ndk standalone toolchain](https://developer.android.com/ndk/guides/standalone_toolchain.html).
 Build toolchain with this command:

    python %ANDROID_NDK%\build\tools\make_standalone_toolchain.py --arch arm --api 16 --install-dir=C:\android_toolchain_arm_api16

After that, just navigate from command line to folder `raylib/src/` and execute Makefile with:

    mingw32-make PLATFORM=PLATFORM_ANDROID

**Remember to setup `ANDROID_NDK`, `ANDROID_TOOLCHAIN` property properly in the Makefile!**

    ANDROID_NDK = C:/android-sdk/ndk-bundle
    ANDROID_TOOLCHAIN = C:/android_toolchain_arm_api16

NOTE: libraylib.a will be generated in folder `raylib/release/android/armeabi-v7a/.

### Compiling a raylib game for Android (using an example project template)

_Step 1._ To build an APK, navigate to folder `raylib/template/simple_game/` and edit Makefile.Android. Replace these
settings with yours where apropriate:

    ANDROID_HOME = C:/android-sdk
    ANDROID_NDK = $(ANDROID_HOME)/ndk-bundle
    ANDROID_TOOLCHAIN = C:/android_toolchain_arm_api16
    ANDROID_BUILD_TOOLS = $(ANDROID_HOME)/build-tools/26.0.3
    ANDROID_PLATFORM_TOOLS = $(ANDROID_HOME)/platform-tools
    JAVA_HOME = C:/PROGRA~1/Java/jdk1.8.0_152

Then build the apk with:
    
    mingw32-make PLATFORM=PLATFORM_ANDROID

_Step 2:_ To install the APK into connected device (previously intalled drivers and activated USB debug mode on device):

    %ANDROID_SDK_TOOLS%\adb install -r simple_game.apk

_Step 4:_ To view log output from device:

    %ANDROID_SDK_TOOLS%\adb logcat -c
    %ANDROID_SDK_TOOLS%\adb -d  logcat raylib:V *:S

Again, Notepad++ comes with an already prepared script to do that. Just open `raylib/templates/simple_game/simple_game.c` and run Notepad++ script `raylib_compile_android_project_run_log`.


**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"