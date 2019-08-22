**NOTE: For alternative instructions, check [Working for Android (on macOS)](https://github.com/raysan5/raylib/wiki/Working-for-Android-(on-macOS)) and [Working for Android (on Linux)](https://github.com/raysan5/raylib/wiki/Working-for-Android-(on-Linux))**

## Installing Android required tools

Android requires a set of tools to do all the build process to generate a .APK game.

### 1. Java 8 JDK (including JRE)

You can just download the JDK from [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and install it or alternatively, you can create a portable install [just decompressing required files](https://www.whitebyte.info/programming/java/how-to-install-a-portable-jdk-in-windows-without-admin-rights).

### 2. Android SDK

Actually, Android SDK is composed by a series of tools, you can install everything in a go just downloading [Android Studio](https://developer.android.com/studio/#downloads) and installing the full package.

Alternatively, you can just install the required files manually. To do that, start downloading `Command line tools` package, found at the end of this [page](https://developer.android.com/studio/#command-tools).

 - Decompress downloaded `sdk-tools-...` file into a folder named `android-sdk`. One of the included tools is the [sdkmanager]((https://developer.android.com/studio/command-line/sdkmanager)), a command line utility to install required packages.
 - From command line, navigate to `android-sdk/tools/bin` and execute the following commands:
```
sdkmanager --update
sdkmanager --list
sdkmanager --install build-tools;28.0.1
sdkmanager --install platform-tools
sdkmanager --install platforms;android-21
sdkmanager --install extras;google;usb_driver
```
With those commands you're installing all required tools. It includes: `tools`, `build-tools`, `platform-tools`, `platforms\android-21` api and also `extras\google\usb_driver` (that you need to install to connect to your device).

### 3. Android NDK

To develop with raylib in C, we need to install the Android Native Development Kit. You can download it from [here](https://developer.android.com/ndk/downloads/). Android NDK includes support files for multiple Android APIs and multiple architectures; it is recommended to generate and standalone toolchain with just the required compiler and API you need. 

Fortunately, Android NDK comes with a Python script that generates it for you, just execute the following command line:

    python %ANDROID_NDK%\build\tools\make_standalone_toolchain.py --arch arm --api 21 --install-dir=C:\android_toolchain_ARM_API21

## Compiling raylib source code

To compile raylib sources, it's recommended to use [Android NDK standalone toolchain](https://developer.android.com/ndk/guides/standalone_toolchain.html). 

Once the toolchain is avaliable, just navigate from command line to folder `raylib/src/` and execute Makefile with:

    mingw32-make PLATFORM=PLATFORM_ANDROID

Remember to setup `ANDROID_TOOLCHAIN` path property in the Makefile!

    ANDROID_TOOLCHAIN = C:/android_toolchain_ARM_API21

NOTE: libraylib.a will be generated in folder `raylib/release/android/armeabi-v7a/` or `raylib/release/android/arm64-v8a/`, depending on the selected architecture (32bit or 64bit).

NOTE: Probably your Android device uses a 64bit CPU but be careful, installed Android OS could still be 32bit, not allowing 64bit apps.

## Compiling a raylib game for Android (using an example project template)

_Step 1._ To build an APK, navigate to folder `raylib/template/simple_game/` and edit Makefile.Android. Replace these
settings with yours where apropriate:
```
ANDROID_ARCH ?= ARM
ANDROID_API_VERSION = 21

JAVA_HOME ?= C:/JavaJDK
ANDROID_HOME = C:/android-sdk
ANDROID_TOOLCHAIN = C:/android_toolchain_$(ANDROID_ARCH)_API$(ANDROID_API_VERSION)
ANDROID_BUILD_TOOLS = $(ANDROID_HOME)/build-tools/28.0.1
ANDROID_PLATFORM_TOOLS = $(ANDROID_HOME)/platform-tools
```
Then build the apk with:
    
    mingw32-make PLATFORM=PLATFORM_ANDROID

_Step 2:_ To install the APK into connected device (previously intalled drivers and activated USB debug mode on device):

    %ANDROID_SDK_TOOLS%\adb install -r simple_game.apk

_Step 4:_ To view log output from device:

    %ANDROID_SDK_TOOLS%\adb logcat -c
    %ANDROID_SDK_TOOLS%\adb -d  logcat raylib:V *:S

**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"

