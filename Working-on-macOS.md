## Building Library

This guide has been written using the following software:
- OSX El Capitan (10.11.3)
- Xcode 7.2.1 (7C1002) 

_Steps:_

1) Get a Mac with OSX version 10.11.3.

2) Install *Apple Developer Tools*. Those tools include Xcode, in our case version 7.2.1. 

3) Install raylib library

##### With Homebrew

- If you don't want to build it yourself, install Homebrew by executing the following command in Terminal.app:  
```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Once Homebrew is installed, run the following command in Terminal:
```
    brew install raylib
```

##### Build newest development snapshot from source

- Download or Clone raylib from GitHub (https://github.com/raysan5/raylib). [`raylib-master.zip`](https://github.com/raysan5/raylib/archive/master.zip) contains all required files: source code, examples, templates, games...
- Decompress `raylib-master.zip` in some folder. In case of using Safari browser, it will be automatically decompressed.
- From Terminal.app, access `raylib-master/src` directory:
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

# Without Xcode - Building Statically 

Building statically means you can run this application on other machines with ease - users won't have to have any of the frameworks installed that are required. Also, this will work on mac's 10.9 and up.

## Here's the quick instructions:

1. From the command line:
`export MACOSX_DEPLOYMENT_TARGET=10.9`
2. Build glfw (With the export command above it will now run on older macs)
By default it builds a dynamic library, and we want static so we need to download the repository from github, and use a special cmake modifier:
````
git clone https://github.com/glfw/glfw.git
cd glfw
cmake -DBUILD_SHARED_LIBS=OFF
make
````
Now, in the src folder of the glfw folder there should be a file called "libglfw3.a"

Confirm that this build matches the version number:
`otool -l libglfw3.a`

You should see a lot of stuff but scan for this:
````
 cmd LC_VERSION_MIN_MACOSX
 cmdsize 16
 version 10.9
````
Okay you're good. Copy glfw/src/libglfw3.a to the folder of your project for easy access.

3. Build raylib (Again, this is so the export line takes affect) 

````
git clone https://github.com/raysan5/raylib.git
cd raylib/src
make
````

You may do the otool check with the file in raylib/src/libs/osx/libraylib.a here if you like. (LC_VERSION_MIN_MACOSX should be version 10.4), and we're good!
copy raylib/src/libs/osx/libraylib.a to your project.

4. Build your project!
```
clang  -framework OpenAL -framework CoreVideo -framework IOKit -framework Cocoa -framework GLUT -framework OpenGL  libraylib.a libglfw3.a my_app.c -o my_app
```

Check for warnings! This can tell you if a library you're linking to was not built for OSX 10.9, in which case you'll need to rebuild that too. 

Check otool one last time for the LC_VERSION_MIN_MACOSX version:
`otool -l my_app`


Right now 1/9/2018, this works with the master branch, but not the development branch.


Last thing, let me show you something cool:

````
otool -L my_app
````

This shows you everything your application links to. Basically, if anything is pointing to anything but /usr/lib/* or /System/Library/*, your application will throw an error if you run it on any other Mac. It's not portable. 
For example if it's linking to something in /usr/local/lib, or a relative folder, that would be bad. But after the above, you should be clear of dynamic dependencies!


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
