**raylib 2.0** includes [Visual Studio 2017 project templates](https://github.com/raysan5/raylib/tree/master/projects/VS2017) for the library and some examples but maybe you want to configure the library for another **Visual Studio** version.

Assuming you are using **Visual Studio 2017** and you downloaded raylib from **github** you can easily follow this step by step guide.

### Configure raylib game project

1. Create a new **Console** project so **File > New > Project...**

2. Go under **Project > Properties of Your Project Name... > C/C++ > General** and include the following additional directories:
    - `$(raylibSrcDir)\release\include`

3. Select **Preprocessor** and include the following preprocessor definitions (for Windows platform):
    - `GRAPHICS_API_OPENGL_33`
    - `PLATFORM_DESKTOP`

4. Under **Advanced** configuration choose: **Compile as C Code (/TC)**

5. Go to **Linker > General** and add the additional directory where **raylib.lib** file is located.

6. Go to **Linker > Input** and add the following additional dependencies:
    - `raylib.lib`

7. Apply the changes and press **Ctrl + Shift + B** for start building your solution.

**Note:** it may be required building **raylib.lib** file for your specific **Visual Studio** version, you can do this with pre-configured [Visual Studio project templates](https://github.com/raysan5/raylib/tree/master/projects/).