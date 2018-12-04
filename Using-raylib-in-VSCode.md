[_VSCode_](https://code.visualstudio.com/) is an excellent choice of code editor when it comes to raylib. Getting set up with a new VSCode project is easy.

### Step 1
Copy the VSCode folder (and all its contents) from raylib/projects/VSCode (from your installed directory) to your desired project location. These files can also be found [here.](https://github.com/raysan5/raylib/tree/master/projects/VSCode). 

**Note**: You can use this [Zip Tool](https://kinolien.github.io/gitzip/) to download only the VSCode folder as a zip.

### Step 2
Make sure you set the proper paths to your local build of raylib in c_cpp_properties.json and tasks.json. These will be specific to your installation of raylib.

in **c_cpp_properties.json** make sure
`"includePath": [
  "C:/raylib/raylib/src/**",
  "${workspaceFolder}/**"
],`

`"compilerPath": "C:/raylib/mingw/bin/gcc.exe",`
in some cases it's **mingw32** instead of **mingw** (which comes with installer v2.0). Check your folder to see which one you have. In **tasks.json** also you have to make this change for compile to occur.

### Step 3
Install the "C/C++" VSCode extension.

### Step 4
Try launching by using the "Debug" launch configuration in the Debug tab.