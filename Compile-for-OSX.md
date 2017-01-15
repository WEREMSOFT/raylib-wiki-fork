This guide has been written using the following software:
- OSX El Capitan (10.11.3) 
- Xcode 7.2.1 (7C1002) 

_Steps:_

1) Get a Mac with OSX version 10.11.3.

2) Install *Apple Developer Tools*. Those tools include XCode, in our case version 7.2.1. 

3) Install GLFW3 library

- To install required packages easier, first install Homebrew, execute in Terminal app the following command:  
```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Once Homebrew app is installed, run the following command in Terminal:
```
    brew install glfw3
```
4)  Install raylib library
- Download or Clone raylib from GitHub (https://github.com/raysan5/raylib). `raylib-master.zip` contains all required files: source code, examples, templates, games...
- Decompress `raylib-master.zip` in some folder. In case of using Safari browser, it will be automatically decompressed.
- From Terminal app, access raylib-master/src directory:
```
    cd raylib-master/src
```
- Compile raylib library using the following command from Terminal:
```
    make PLATFORM=PLATFORM_DESKTOP
```
- If everything worked ok, `libraylib.a` should be created in `raylib-master/release/osx` folder.

5) Add generated libraries (raylib, glfw3) to XCode project.
- Create a new XCode project using `Command Line Tool`. Make sure selected language is C.
- Once project created and open, Mouse click over the project main folder in the left project-navigation panel. It should appear `Build Phases` window, just enter and select `Link Binary With Libraries`. There you should add project libraries:
- To add OpenGL and OpenAL: Click on + and add OpenGL.framework and OpenAL.framework
- To add raylib: Click on + and `Add Other...`, look for `libraylib.a` file created previously, it should be in folder `raylib-master/release/osx` (make sure library has been created in that folder).
- To add GLFW3: Click on + and `Add Other...`, look for folder `/usr/local/lib` and look for file `libglfw3(version).dylib`. 
- Make sure XCode finds `raylib.h`: Go to `Build Settings > Search Paths` and add raylib header folder (`raylib-master/src`) to `Header Search Paths` 
- Make sure XCode finds `libraylib.a`: Go to `Build Settings > Search Paths` and add raylib library folder (`raylib-master/release/osx`) to `Library Search Paths`.

6) raylib should work correctly. To make sure, just go to [official raylib page](www.raylib.com) and check the different examples available. Just copy the code into `main.c` file and run it with Run button or ⌘R.

_NOTES:_

- It seems there is a problem with HiDPI displays, in that case, app Window appears smaller. Solution is just moving a bit the Window and it should get scaled automatically.
- Examples resources should be placed in the folder where XCode generates the product.

_Tutorial written by Aleix Rafegas and trasnlated to english by Ray_