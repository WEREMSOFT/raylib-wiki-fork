Besides the `games/` and `examples/` that are built along raylib by default, there's also a sample CMake project at `projects/CMake`:

```
cd projects/CMake # You can also just download the CMakeLists.txt and the core_basic_window.c file separately
mkdir build       # Create an out-of-tree build directory. That way build artifacts aren't mixed with source code
cd build          # enter it
cmake ..          # run cmake on the parent directory
cmake --build .   # kick of the build process
```

The `CMakeLists.txt` is self contained and will arrange to probe whether raylib has been installed and if not, it's downloaded, built and linked statically into the `core_basic_window` example application.

## Use from within an IDE

CMake supports a range of generators, which can be used to generate project files for IDEs/Build-Tools such as Visual Studio, Ninja or Sublime Text 2. e.g. for Xcode you can run `cmake -G 'Xcode' ..` to have it generate project files for import into Xcode.