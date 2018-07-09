To build your raylib game for Android you need a set of Android tools. Those tools are basically the Android SDK and Android NDK, including Android Platform Tools and also Java Runtime.

**NOTE: For alternative instructions, check [Working for Android (on macOS)](https://github.com/raysan5/raylib/wiki/Working-for-Android-(on-macOS)).**

### Installing Android required tools

1. [Java 8 JDK and JRE](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
2. Android SDK, just required the [command line tools](https://developer.android.com/studio/#downloads)

  We will use the [SDKManager](https://developer.android.com/studio/command-line/sdkmanager) to install all required tools

3. [Android NDK](https://developer.android.com/ndk/downloads/)

### Compiling raylib source code

To compile raylib sources, it's recommended to use [Android NDK standalone toolchain](https://developer.android.com/ndk/guides/standalone_toolchain.html).

You can build the toolchain with this command:

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

**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"

