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