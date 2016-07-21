To build your raylib game for Oculus Rift CV1, raylib installer comes with all the required tool; it includes a ready to use code editor: Notepad++ with some **useful scripts** ready to use and it also includes a compiler with basic C libraries: MinGW.

If you want to use another code editor like Visual Studio or another compiler package you can also use them but the instructions in this page are focused on raylib standard development tools: Notepad++ and MinGW.

###Installing raylib

Just download raylib installer and run it (full installation). It installs custom versions of Notepad++ and MinGW, prepared to work with raylib. Source code also comes with Oculus Rift required libraries.

NOTE: raylib 1.5 comes with Oculus Rift runtime libraries v1.6.0; the [Oculus PC SDK](https://developer.oculus.com/downloads/pc/1.6.0/Oculus_SDK_for_Windows/) is updated quite frequently. If you need to update to latest version and create a game using latest SDK version features, you can just update [raylib/src/external/OculusSDK/LibOVR](https://github.com/raysan5/raylib/tree/master/src/external/OculusSDK/LibOVR) in your installation folder.

###Compiling raylib source code

_Step 1:_ By default, raylib compiled version does not include Oculus Rift support, it must be recompiled including Oculus libraries. To do that, just open Notepad++, open `raylib/src/core.c` and execute Notepad++ script:

    raylib_source_compile_gl33_oculus_rift

NOTE: raylib will be compiled for OpenGL 3.3 Core backend with Oculus Rift support and all required files (library, headers) will be automatically copied to required folders. Just keep in mind that if you're not developing for Oculus Rift, you should use non-oculus version of the library. Get back to that version executing Notepad++ script:

    raylib_source_compile_gl33

###Compiling your raylib Oculus Rift game

_Step 1:_ Just open on Notepad++ your Oculus Rift game file (check `raylib/examples/core_oculus_rift.c` for reference) and execute Notepad++ script:

    raylib_compile_oculus_rift

NOTE: To actually see the example running in the device, device must be ready (Oculus Desk screen) and you have to wear the device (or the sensor in the middle-top of the eyes must be blocked).