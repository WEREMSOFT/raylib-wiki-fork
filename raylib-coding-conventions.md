This page list some coding conventions rules I try to follow on raylib C code:

Code element | Convention | Example
--- | :---: | ---
Macros | ALL_CAPS | `#define MIN(a,b) (((a)<(b))?(a):(b))`
Defines | ALL_CAPS | `#define PLATFORM_DESKTOP`
Constants | ALL_CAPS | `const int NUMBER = 8`
Struct | TitleCase | `struct Texture2D`
Struct members |lowerCase | `texture.id`
Enum | TitleCase | `TextureFormat`
Enum members | ALL_CAPS | `UNCOMPRESSED_R8G8B8`
Functions | TitleCase or prefixTitleCase | `InitWindow()`
Variables | lowerCase | `screenWidth`
Local variables | lowerCase | `playerPosition`
Global variables | lowerCase | `fullscreen`
Operators | value1*value2 | `int product = value*6;`
Operators | value1/value2 | `int division = value/4;`
Operators | value1 + value2 | `int sum = value + 10;`
Operators | value1 - value2 | `int res = value - 5;`
Pointers | MyType *pointer | `Texture2D *array;`
float values | always x.xf | `float value = 10.0f`
