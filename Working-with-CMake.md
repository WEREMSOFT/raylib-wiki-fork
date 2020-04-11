## Requiring raylib to be installed
If you know that anyone who builds your project, has raylib already installed, a simple `CMakeLists.txt` could look like this:
```cmake
cmake_minimum_required(VERSION 3.15)
project(my_project)

find_package(raylib 2.5.0 REQUIRED) # Requires at least version 2.5.0

set(CMAKE_C_STANDARD 11)

add_executable(my_project main.c)

target_link_libraries(my_project main raylib)
```
To build, use these commands:
```cmake
mkdir build # Create a build directory
cd build && cmake .. # Build from that directory so the build files are in one place
cmake --build . # Build the project
```

## Loading raylib inside cmake
If you want someone who builds your project to be able to download and build raylib with it, the [CMakeLists.txt at projects/CMake](https://github.com/raysan5/raylib/blob/master/projects/CMake/CMakeLists.txt)
 will help you.

## Use from within an IDE

CMake supports a range of generators, which can be used to generate project files for IDEs/Build-Tools such as Visual Studio, Ninja or Sublime Text 2. e.g. for Xcode you can run `cmake -G 'Xcode' ..` to have it generate project files for import into Xcode.