# Building raylib for Android on MAC OSX

It's helpful to know step by step what each part of the process is doing. For this, let's create a simple bash script, where each line can be run manually and the results can be seen. This should be useful for understanding the process on Linux as well.

This is simple as in we're just using shell commands, no cmake, no builders of any kind. 

## Initial folder setup

> Note: project.c is copied from raylib/templates/simple_game/simple_game.c

    /toolchain_arm_api22    put android_toolchain_arm_api22 here
    /raylib                 put raylib here
    /project1/lib
    /project1/obj
    /project1/src
    /project1/dex
    /project1/res/values/strings.xml
    /project1/assets
    /project1/project.c
    /project1/AndroidManifest.xml

### /res/values/strings.xml

> Note: This is likely not necessary.
  
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <string name="app_name">My App</string>
    </resources>

### /src/com/seth/project/NativeLoader.java

> Note: My project is named "project" and the namespace is named "seth"

    package com.seth.project; 
    public class NativeLoader extends android.app.NativeActivity { 
        static {
            System.loadLibrary("project"); // must match name of shared library (in this case libproject.so) 
        } 
    } 

AndroidManifest.xml

> Note: "package" is the unique name of your project, this will overwrite apps with the same name

    <?xml version='1.0' encoding="utf-8" ?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" package='com.seth.project' android:versionCode='0'
        android:versionName='0' >
        <application>
            <activity android:name="com.seth.project.NativeLoader"
                android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
                android:configChanges="orientation|keyboardHidden|screenSize"
                android:launchMode="singleTask"
                android:clearTaskOnLaunch="true">
            <meta-data android:name="android.app.lib_name" android:value="project"/>
                <intent-filter>
                    <category android:name='android.intent.category.LAUNCHER'/>
                    <action android:name='android.intent.action.MAIN'/>
                </intent-filter>
            </activity>
        </application>
    </manifest>

## Outline of the Process

    cp              Copy libs from raylib to lib
    gcc             Gen native_app_glue.o from raylib
    ar rcs          Gen obj/libnative_app_glue.a from native_app_glue.o
    gcc             Gen project.o from project.c
    gcc             Gen lib/libproject.so from obj/project.o
    aapt package    Gen R.java
    javac           Gen classes from R.java and NativeLoader.java
    dx              Gen classes.dex from objects
    aapt package    Gen project.unsigned.apk
    aapt add        Add shared library to project.unsigned.apk
    keytool         Generate keystore
    jarsigner       Gen project.signed.apk 
    zipalign        Align project
    adb install     Install project
    adb logcat      view log

## Install Standalone Toolchain

Install ndk-bundle using Android Studio. Install this in t
    
    ~/Library/Android/sdk/ndk-bundle/build/tools/make-standalone-toolchain.sh --arch=arm --platform=android-22 --install-dir=./toolchain_arm_api22

## Install raylib

    Compile Raylib into ./raylib

## ~/.bash_profile

> Add these files to path so you won't have a big headache later on

    export ANDROID_SDK_ROOT="/Users/watchmyfeet/Library/Android/sdk"
    export PATH="${ANDROID_SDK_ROOT}/build-tools/27.0.3/:$PATH"
    export PATH="${ANDROID_SDK_ROOT}/tools:$PATH"
    export PATH="${ANDROID_SDK_ROOT}/tools/bin:$PATH"
    export PATH="${ANDROID_SDK_ROOT}/platform-tools:$PATH"

## Initial Project Setup
 
> Note: Keytool is in java folder on system

    cd project1
    mkdir lib/armeabi-v7a
    cp ../raylib/release/libs/android/armeabi-v7a/libraylib.a lib/armeabi-v7a/libraylib.a
    keytool -genkeypair -validity 1000 -dname "CN=seth,O=Android,C=ES" -keystore project.keystore -storepass 'whatever' -keypass 'mypass' -alias projectKey -keyalg RSA

## Build Script

> Note: Here's step by step commands which lead to the installation of your project!

    ../toolchain_arm_api22/bin/arm-linux-androideabi-gcc -c ../raylib/src/external/android/native_app_glue/android_native_app_glue.c -o obj/native_app_glue.o -std=c99 -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -ffunction-sections -funwind-tables -fstack-protector-strong -fPIC -Wall -Wa,--noexecstack -Wformat -Werror=format-security -no-canonical-prefixes -DANDROID -DPLATFORM_ANDROID -D__ANDROID_API__=22

    # Requires: folder setup
    # Creates: obj/native_app_glue.o
    # Note: This gcc uses other tools in the same toolchain folder structure, don't even thing about symlinking to it.

    ../toolchain_arm_api22/bin/arm-linux-androideabi-ar rcs obj/libnative_app_glue.a obj/native_app_glue.o

    # Requires: obj/native_app_glue.o
    # Creates: obj/libnative_app_glue.a

    ../toolchain_arm_api22/bin/arm-linux-androideabi-gcc -c project.c -o obj/project.o -I. -I../raylib/release/include -I../raylib/src/external/android/native_app_glue -std=c99 -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -ffunction-sections -funwind-tables -fstack-protector-strong -fPIC -Wall -Wa,--noexecstack -Wformat -Werror=format-security -no-canonical-prefixes -DANDROID -DPLATFORM_ANDROID -D__ANDROID_API__=22 --sysroot=../toolchain_arm_api22/sysroot

    # Requires: project.c
    # Creates: obj/project.o

    ../toolchain_arm_api22/bin/arm-linux-androideabi-gcc -o lib/armeabi-v7a/libproject.so obj/project.o -shared -I. -I../raylib/release/include -I../raylib/src/external/android/native_app_glue -Wl,-soname,libproject.so -Wl,--exclude-libs,libatomic.a -Wl,--build-id -Wl,--no-undefined -Wl,-z,noexecstack -Wl,-z,relro -Wl,-z,now -Wl,--warn-shared-textrel -Wl,--fatal-warnings -u ANativeActivity_onCreate -L. -Lobj -Llib/armeabi-v7a -lraylib -lnative_app_glue -llog -landroid -lEGL -lGLESv2 -lOpenSLES -latomic -lc -lm -ldl

    # Requires: obj/project.o
    # Creates: lib/armeabi-v7a/libproject.so

    aapt package -f -m -S res -J src -M AndroidManifest.xml -I ${ANDROID_SDK_ROOT}/platforms/android-22/android.jar

    # Requires: AndroidManifest.xml, res/
    # Creates: src/com/seth/project/R.java

    # javac -verbose -source 1.7 -target 1.7 -d obj -bootclasspath `/usr/libexec/java_home`/jre/lib/rt.jar -classpath ${ANDROID_SDK_ROOT}/platforms/android-22/android.jar:obj -sourcepath src src/com/seth/project/R.java src/com/seth/project/NativeLoader.java

    # Requires: src/com/seth/project/R.java, src/com/seth/project/NativeLoader.java
    # Creates: obj/com/seth/project/NativeLoader.class ... R&attr.class R$string.class R.class 

    dx --verbose --dex --output=dex/classes.dex obj

    # Requires: obj/com/seth/project/NativeLoader.class ... R&attr.class R$string.class R.class 
    # Creates: dex/classes.dex

    aapt package -f -M AndroidManifest.xml -S res -A assets -I ${ANDROID_SDK_ROOT}/platforms/android-22/android.jar -F project.unsigned.apk dex

    # Creates: project.unsigned.apk
    # Note: The "dex" at the end is the directory the classes.dex file is in! This folder can not contain the manifest file for whatever reason.

    aapt add project.unsigned.apk lib/armeabi-v7a/libproject.so

    # Does: Adds shared library to apk
    
    jarsigner -keystore project.keystore -storepass whatever -keypass mypass -signedjar project.signed.apk project.unsigned.apk projectKey

    # Does: Signs

    zipalign -f 4 project.signed.apk project.apk

    # Does: Aligns
    
    adb install -r project.apk

    # Does: install

## Error: couldn't find libprojectlibrary.so

You'll get this if your shared library's name does not match across the build. In my case, I was referencing projectlibrary in the java file where it should have just been "project"

## Error: INSTALL_FAILED_NO_MATCHING_ABIS

This one was tricky! Make sure the platform of the standalone toolchain you build matches the platform you are using on your system.

    make-standalone-toolchain.sh --arch=arm --platform=android-22 --install-dir=./toolchain_arm_api22
    ${ANDROID_SDK_ROOT}/platforms/android-22

This command may also help.

    adb shell getprop ro.product.cpu.abi

## Error: INSTALL_FAILED_DEXOPT

This one was easier. The Apk generated must contain classes/classes.dex. If it doesn't, check aapt package -f -M to make sure you are including the folder dex is in at the end of it.

## Dealing with Segmentation Faults - Using adb logcat

In the event of a segfault, don't despair! You can get the entire backtrace using adb logcat.

```
adb logcat "libc:*" "DEBUG:*" "*:S"
```
This gives you everything tagged with libc or DEBUG. "*:S" is there to Silence all other stuff.

Now, you may see a message that says "Suppressing debuggerd output because prctl(PR_GET_DUMPABLE)==0"

In that case you have to set this to 1.

In your c code, dd this include statement, and the prctl command somewhere early on in your main():

```
#include <sys/prctl.h>
...
prctl(PR_SET_DUMPABLE, 1, 0, 0, 0);
```

Here's a more robust logcat command:

```
adb logcat "threaded_app:*" "raylib:*" "libc:*" "DEBUG:*" "InputDispatcher:*" "JavaBinder:*" "WindowState:*" "Eve:*" "ViewRootImpl:*" "*:S"
```

Or be brave and try to find the important tags yourself:
```
adb logcat "*:*"
```
