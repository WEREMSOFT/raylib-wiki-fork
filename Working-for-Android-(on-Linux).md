## NOTE: This guide is tested on Ubuntu 18.04 and Android 6.0.1
# Installing required tools
***

We need a set of tools in order to generate an .apk file. These tools are nothing more than couple things. Build process is relatively simple. It is done by shell commands and Android tools.
## 1- Java 8 JDK
You can get Java 8 JDK in multiple ways. I'm using OpenJDK at this guide. Follow the instructions at https://openjdk.java.net/ address to install OpenJDK.
## 2- Android SDK
Android SDK has many packages. If you want to download all of them, you can go install [Android Studio](https://developer.android.com/studio/) and install the full package. But if you don't, you can download only the required ones manually by [Command Line Tools](https://developer.android.com/studio/#command-tools). To do that, start downloading the [Command Line Tools](https://developer.android.com/studio/#command-tools).
* Decompress downloaded `sdk-tools-linux-...` into a folder named `android-sdk`.
* From the Terminal, navigate to `android-sdk/tools/bin` and run the following commands:

```
./sdkmanager --update
./sdkmanager --list
./sdkmanager --install "build-tools;28.0.1"
./sdkmanager --install platform-tools
./sdkmanager --install "platforms;android-22"
```
All required tools will be downloaded by these commands. You can check out package's description by looking at `./sdkmanager --list`.
## 3- Android NDK
Raylib is written in C, hence we need Android NDK. Android Native Development Kit (NDK) allows us to develop in C. You can download it from [here](https://developer.android.com/ndk/downloads/). In this case we need Android NDK because we are going to use it's compiler and API with a standalone toolchain. 
After downloading Android NDK
* Decompress downloaded `android-ndk-...` and rename it as `android-ndk`
* From Terminal navigate to `android-ndk/build/tools`
* Run `python make_standalone_toolchain.py --arch arm --api 22 --install-dir=android_toolchain_ARM_API22`

# Compiling Raylib
***

First you should download Raylib source code. Run following command from Terminal:
`git clone https://github.com/raysan5/raylib.git raylib`
Source code will be downloaded to a directory named raylib.
We need to compile Raylib with Android NDK Standalone Toolchain that we made in the previous section.
Don't forget to setup your `ANDROID_TOOLCHAIN` path and `ANDROID_NDK` path in your Makefile!(`raylib/src/Makefile`)

Default property for `ANDROID_TOOLCHAIN` is `ANDROID_TOOLCHAIN = C:/android_toolchain_$(ANDROID_ARCH)_API$(ANDROID_API_VERSION)`

Default property for `ANDROID_NDK` is `ANDROID_NDK = C:/android-ndk`

After that navigate from Terminal to `raylib/src` and, compile Raylib with `make PLATFORM=PLATFORM_ANDROID`

This command will create `libraylib.a` in `/src`.

# Initial Setup For Your Raylib app
***
## Folders
Actually, we can add this part to a `.sh` file but to clarify building process, we are going to create all necessary files manually.
> Note: project.c is copied from `raylib/templates/simple_game/simple_game.c`
```
/toolchain_arm_api22    put android_toolchain_arm_api22 here
/raylib                 put raylib here
/project/lib
/project/obj
/project/src
/project/dex
/project/res/values/strings.xml
/project/assets
/project/project.c
/project/AndroidManifest.xml
```
## Files
### project/res/values/strings.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">My App</string>
</resources>
```
### /src/com/raylib_app/project/NativeLoader.java
> Note: My project name is `project` and my namespace is named `raylib_app`. You can change it however you want.
```
package com.raylib_app.project; 
public class NativeLoader extends android.app.NativeActivity { 
    static {
        System.loadLibrary("project"); // must match name of shared library (in this case libproject.so) 
    } 
} 
```
### AndroidManifest.xml
```
<?xml version='1.0' encoding="utf-8" ?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package='com.raylib_app.project' android:versionCode='0'
    android:versionName='0' >
    <application>
        <activity android:name="com.raylib_app.project.NativeLoader"
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
```
### lib/armeabi-v7a/libraylib.a
```
cd project
mkdir lib/armeabi-v7a
cp ../raylib/src/libraylib.a lib/armeabi-v7a/libraylib.a
keytool -genkeypair -validity 1000 -dname "CN=seth,O=Android,C=ES" -keystore project.keystore -storepass 'whatever' -keypass 'whatever' -alias projectKey -keyalg RSA
```
# Build Script
```
../android_toolchain_ARM_API22/bin/arm-linux-androideabi-gcc -c ../raylib/src/external/android/native_app_glue/android_native_app_glue.c -o obj/native_app_glue.o -std=c99 -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -ffunction-sections -funwind-tables -fstack-protector-strong -fPIC -Wall -Wa,--noexecstack -Wformat -Werror=format-security -no-canonical-prefixes -DANDROID -DPLATFORM_ANDROID -D__ANDROID_API__=22

# Requires: folder setup
# Creates: obj/native_app_glue.o
# Note: This gcc uses other tools in the same toolchain folder structure, don't even thing about symlinking to it.

../android_toolchain_ARM_API22/bin/arm-linux-androideabi-ar rcs obj/libnative_app_glue.a obj/native_app_glue.o

# Requires: obj/native_app_glue.o
# Creates: obj/libnative_app_glue.a

../android_toolchain_ARM_API22/bin/arm-linux-androideabi-gcc -c project.c -o obj/project.o -I. -I../raylib/release/include -I../raylib/src/external/android/native_app_glue -std=c99 -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -ffunction-sections -funwind-tables -fstack-protector-strong -fPIC -Wall -Wa,--noexecstack -Wformat -Werror=format-security -no-canonical-prefixes -DANDROID -DPLATFORM_ANDROID -D__ANDROID_API__=22 --sysroot=../toolchain_ARM_API22/sysroot

# Requires: project.c
# Creates: obj/project.o

../android_toolchain_ARM_API22/bin/arm-linux-androideabi-gcc -o lib/armeabi-v7a/libproject.so obj/project.o -shared -I. -I../raylib/release/include -I../raylib/src/external/android/native_app_glue -Wl,-soname,libproject.so -Wl,--exclude-libs,libatomic.a -Wl,--build-id -Wl,--no-undefined -Wl,-z,noexecstack -Wl,-z,relro -Wl,-z,now -Wl,--warn-shared-textrel -Wl,--fatal-warnings -u ANativeActivity_onCreate -L. -Lobj -Llib/armeabi-v7a -lraylib -lnative_app_glue -llog -landroid -lEGL -lGLESv2 -lOpenSLES -latomic -lc -lm -ldl

# Requires: obj/project.o
# Creates: lib/armeabi-v7a/libproject.so

ANDROID_SDK_PATH/build-tools/28.0.1/aapt package -f -m -S res -J src -M AndroidManifest.xml -I ANDROID_SDK_PATH/platforms/android-22/android.jar

# Requires: AndroidManifest.xml, res/
# Creates: src/com/raylib_app/project/R.java

javac -verbose -source 1.7 -target 1.7 -d obj -bootclasspath /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/jre/lib/rt.jar -classpath ANDROID_SDK_PATH/platforms/android-22/android.jar:obj -sourcepath src src/com/raylib_app/project/R.java src/com/raylib_app/project/NativeLoader.java

# Requires: src/com/raylib_app/project/R.java, src/com/raylib_app/project/NativeLoader.java
# Creates: obj/com/raylib_app/project/NativeLoader.class ... R&attr.class R$string.class R.class 

ANDROID_SDK_PATH/build-tools/28.0.1/dx --verbose --dex --output=dex/classes.dex obj 

# Requires: obj/com/raylib_app/project/NativeLoader.class ... R&attr.class R$string.class R.class 
# Creates: dex/classes.dex

ANDROID_SDK_PATH/build-tools/28.0.1/aapt package -f -M AndroidManifest.xml -S res -A assets -I ANDROID_SDK_PATH/platforms/android-22/android.jar -F project.unsigned.apk dex

# Creates: project.unsigned.apk
# Note: The "dex" at the end is the directory the classes.dex file is in! This folder can not contain the manifest file for whatever reason.

ANDROID_SDK_PATH/build-tools/28.0.1/aapt add project.unsigned.apk lib/armeabi-v7a/libproject.so

# Does: Adds shared library to apk

jarsigner -keystore project.keystore -storepass whatever -keypass whatever -signedjar project.signed.apk project.unsigned.apk projectKey

# Does: Signs

ANDROID_SDK_PATH/build-tools/28.0.1/zipalign -f 4 project.signed.apk project.apk

# Does: Aligns

ANDROID_SDK_PATH/platform-tools/adb install -r project.apk

# Does: install (on an Android phone that you connect with USB Debugging)
```
It is all done here. You can change the build script as your preferences. Have fun!