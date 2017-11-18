This guide has been written using the following software:
- OSX El Capitan (10.11.3) 
- Xcode 7.2.1 (7C1002) 

_Steps:_

1) Get a Mac with OSX version 10.11.3.

2) Install *Apple Developer Tools*. Those tools include Xcode, in our case version 7.2.1. 

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

5) Add generated libraries (raylib, glfw3) to Xcode project.
- Create a new Xcode project using `Command Line Tool`. Make sure selected language is C.
- Once project created and open, Mouse click over the project main folder in the left project-navigation panel. It should appear `Build Phases` window, just enter and select `Link Binary With Libraries`. There you should add project libraries:
- To add OpenGL and OpenAL: Click on + and add OpenGL.framework and OpenAL.framework
- To add raylib: Click on + and `Add Other...`, look for `libraylib.a` file created previously, it should be in folder `raylib-master/release/osx` (make sure library has been created in that folder).
- To add GLFW3: Click on + and `Add Other...`, look for folder `/usr/local/lib` and look for file `libglfw3(version).dylib`. 
- Make sure Xcode finds `raylib.h`: Go to `Build Settings > Search Paths` and add raylib header folder (`raylib-master/src`) to `Header Search Paths` 
- Make sure Xcode finds `libraylib.a`: Go to `Build Settings > Search Paths` and add raylib library folder (`raylib-master/release/osx`) to `Library Search Paths`.

6) raylib should work correctly. To make sure, just go to [official raylib page](http://www.raylib.com) and check the different examples available. Just copy the code into `main.c` file and run it with Run button or ⌘R.

_NOTES:_

- It seems there is a problem with HiDPI displays, in that case, app Window appears smaller. Solution is just moving a bit the Window and it should get scaled automatically.
- Examples resources should be placed in the folder where Xcode generates the product.

_Tutorial written by Aleix Rafegas and translated to English by Ray_

# Without Xcode

1) Install GLFW3 library

- To install required packages easier, first install Homebrew, execute in Terminal app the following command:  
```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Once Homebrew app is installed, run the following command in Terminal:
```
    brew install glfw3
```

2) Complete step 4 in the Xcode section above (about installing raylib).

3) Setup build script

You can create a build.sh file that you can run to compile your project. In the example below your project is named my_app.cpp (for C++) and compiles to my_app.

````
clang++ -I /path/to/raylib/src -L /usr/local/lib -L /path/to/raylib/release/libs/osx -lglfw -lraylib -framework GLUT -framework OpenGL -framework Cocoa my_app.cpp -o my_app
````

If you'd like to use C instead:
````
clang -I /path/to/raylib/src -L /usr/local/lib -L /path/to/raylib/release/libs/osx -lglfw -lraylib -framework GLUT -framework OpenGL -framework Cocoa my_app.c -o my_app
````

Make sure to replace `/path/to/raylib` with the actual path to your cloned copy of the raylib repository.

Now running build.sh (after setting permissions, ie. `chmod +x build.sh`) will compile your program!

Note that this may give you a CLITERAL error, which seems to be an issue in raylib.h: https://github.com/raysan5/raylib/blob/develop/src/raylib.h#L261

Simply comment out that section so that only this line is active:
````
    #define CLITERAL    (Color)
````
..Or just don't use CLITERAL and use (Color){255,255,255,255} instead. (That would be all white)


# Building Statically, so you can Run on Other Computers

Let me show you something cool. 

````
otool -L my_app
````

This shows you everything your application links to. Basically, if anything is pointing to anything but /usr/lib/* or /System/Library/*, your application will throw an error if you run it on any other Mac. It's not portable. Right now, perhaps it's linking to something in /usr/local/lib, or a relative folder. This is bad. We must fix.

Another thing to observe:

````
otool -l my_app
````

Whoa that was a bunch of stuff. Disregard most of it and try to find "LC_VERSION_MIN_MACOSX", look below and find the version number, this will likely be the version of the computer you are currently on. That means anyone on an older OS will receive an error. Again: Bad. We Fix.

First things first, let's make sure we get a reasonable amount of playability on older versions of MacOS. Executing this code before compiling packages, will make sure that your program will run without error on Macs 10.9 and up. (Perhaps you can do older, but you get a warning at 10.8 and lower when compiling)

````
export MACOSX_DEPLOYMENT_TARGET=10.9
````

You'll need to rebuild raylib + glfw for the above to affect anything, fortunately that's what we're doing next.

Next, let's make sure we are statically generating the build. This pulls in raylib and glfw, so that your computer isn't seeking these libraries out dynamically during run time.

Perhaps this isn't required - I suppose you can just pull in the dylib from a relative directory when you package your executeable in a bundle, but this is what I got to work for now.

Unfortunately, when you built glfw in a step above using Homebrew, it builds a dylib, not a static .a file. I had to clone from github and then build the library from scractch using cmake -DBUILD_SHARED_LIBS=OFF

Let's confirm MACOSX_DEPLOYMENT_TARGET did it's thing. 

````
otool -l libglfw3.a
````

Check under LC_VERSION_MIN_MACOSX for version. It should read 10.9

Rebuild raylib, so the OSX version command above takes effect, and confirm.

````
otool -l libraylib.a
````
  
All good? Good.
Once that's done, copy libglfw3.a and librarylib.a into the root directory, and you can link to it like so:

````
clang -I/w/raylib_build/raylib/release/osx -l./libraylib.a -l./libglfw3.a -framework GLUT -framework OpenGL -framework Cocoa my_app.c -o my_app
````

Check for warnings! This can tell you if a library you're linking to was not built for OSX 10.9, in which case you'll need to rebuild that too. 

Compare your linker results from the first time:

````
otool -L my_app
````
This should be all /usr/lib/* or /System/Library/*.

Let's check the version of OSX again:
````
otool -l my_app
````
Again, check under LC_VERSION_MIN_MACOSX for version. It should read 10.9

# Bundle your app in an Application

````
mkdir standard.app/Contents
mkdir standard.app/Contents/MacOS
mkdir standard.app/Contents/Resources
touch standard.app/Contents/Info.plist
````

The app you just created, "my_app" should go in the MacOS folder.

````
mv my_app standard.app/Contents/MacOS
````


Info.plist should read like this:
````
<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
      <key>CFBundleExecutable</key>
      <string>my_app</string>
    </dict>
    </plist>
````

See more fields you can add here:     https://stackoverflow.com/questions/1596945/building-osx-app-bundle

Now you can double click on standard.app and it will run your application!
Note that some things will be cached by the OS. If you want to refresh your application bundle run this:

````
/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -f standard.app
````

This has a whole lot of potentially useful info on all the apps on your system, you can use this to determine if the version is correct I suppose:

````
 /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -dump > dump.txt
````

Just search for your app in dump.txt.

# Creating a DMG image for sharing your app

You could just as easily do a zip I suppose, but DMGs are fashionable aren't they?

Here's a 32 megabyte dmg:
````
    hdiutil create -size 32m -fs HFS+ -volname "My App" my_app_writeable.dmg
    hdiutil attach test.dmg
````

This should tell you something like /dev/disk3 or something. Make a note of that, you'll need it.

Drag your app into the dmg. Then run this, replacing disk999 with whatever /dev/disk was specified.
````
    hdiutil detach /dev/disk999
    hdiutil convert my_app_writeable.dmg -format UDZO -o my_app.dmg
````
There you go. my_app.dmg is ready to be sent to all your most trusted game critics.