To build your raylib game for Android you need a different compiler than the one that comes with raylib installation, actually, you need a set of tools. Those tools are basically the Android SDK and Android NDK.

###Installing Android tools

_Step 1._ Install Java Runtime 8 (JRE), Android SDK, Android NDK and Apache Ant tools:

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

###Compiling raylib source code

Navigate from command line to folder `raylib/src_android/` and type:

    %ANDROID_NDK_ROOT%\ndk-build

NOTE: libraylib.a will be generated in folder `raylib/src_android/obj/local/armeabi/`, it must be copied
to Android project; if using `raylib/template_android` project, copy it to `raylib/template_android/jni/libs/`.

You can also use Notepad++ prepared script for raylib source compilation. Just open Notepad++, open `raylib/src_android/jni/Android.mk` and execute Notepad++ script: `raylib_source_compile_android`.

###Compiling raylib game for Android (using project template)

_Step 1._ To compile project, navigate from command line to folder `raylib/template/android_project/jni/` and type:

    %ANDROID_NDK_ROOT%\ndk-build

_Step 2._ To generate APK, navigate to folder `raylib/template/android_project/jni/` and type:

    %ANT_HOME%\bin\ant debug

_Step 3:_ To install APK into connected device (previously intalled drivers and activated USB debug mode on device):

    %ANT_HOME%\bin\ant installd

_Step 4:_ To view log output from device:

    %ANDROID_SDK_TOOLS%\adb logcat -c
    %ANDROID_SDK_TOOLS%\adb -d  logcat raylib:V *:S

Again, Notepad++ comes with an already prepared script to do that. Just open `raylib/templates/android_project/jni/basic_game.c` and run Notepad++ script `raylib_compile_android_project_run_log`.


**If you have any doubt, [just let me know][raysan5].**

[raysan5]: mailto:raysan5@gmail.com "Ramon Santamaria - Ray San"